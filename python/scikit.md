# Valid Scikit Estimator

1. Init variables much be of a data type which evaluates as equal when compared with another copy of itself
	- pass the string of the estimator and use a method to resolve the name
2. Fit method needs to return the estimator itself for chaining
3. Attribute `self.is_fitted_` is set by `.fit()` method and checked by `.predict()`
4. Input is validated with `check_array()`


```python
def split_left_right_censored(x, y, cens):
	counts = cens.value_counts() 
	if -1 not in counts and 1 not in counts: 
		warnings.warn("No censored observations; use regression methods for uncensored data")
	xs = [] 
	ys = [] 

	for value in [-1, 0, 1]:
		if value in counts:
			split = cens == value 
			y_split = np.squeeze(y[split].values)
			x_split = x[split].values 

		else: 
			y_split, x_split = None, None 
		xs.append(x_split)
		ys.append(y_split)
	return xs, ys 

def tobit_neg_log_likelihood(xs, ys, params):
	x_left, x_mid, x_right = xs 
	y_left, y_mid, y_right = ys 

	b = params[:-1]
	s = params[-1]

	to_cat = []

	cens = False 

	if y_left is not None:
		cens = True 
		left = (y_left - np.dot(x_left, b))
		to_cat.append(left)

	if y_right is not None:
		cens = True
		right = (np.dot(x_right, b) - y_right)
		to_cat.append(right)

	if cens:
		concat_stats = np.concatenate(to_cat, axis=0) / s 
		log_cum_norm =scipy.stats.norm.logcdf(concat_stats)
		cens_sum = log_cum_norm.sum()
	else:
		cens_sum = 0

	if y_mid is not None:
		mid_stats = (y_mid - np.dot(x_mid, b)) / s
		mid = scipy.stats.norm.logpdf(mid_stats) - math.log(max(np.finfo('float').resolution, s))
		mid_sum = mid.sum() 
	else:
		mid_sum = 0
	
	loglik = cens_sum + mid_sum 
	return - loglik

def tobit_neg_log_likelihood_der(xs, ys, params):
	x_left, x_mid, x_right = xs 
	y_left, y_mid, y_right = ys 

	b = params[:-1]
	s = params[-1]

	beta_jac = np.zeros(len(b))
	sigma_jac = 0

	if y_left is not None:
		left_stats = (y_left - np.dot(x_left, b)) / s
		l_pdf = scipy.stats.norm.logpdf(left_stats)
		l_cdf = log_ndtr(left_stats)
		left_frac = np.exp(l_pdf - l_cdf)
		beta_left = np.dot(left_frac, x_left / s)
		beta_jac -= beta_left

		left_sigma = np.dot(left_frac, left_stats)
		sigma_jac -= left_sigma

	if y_right is not None:
		right_stats = (np.dot(x_right, b) - y_right) / s
		r_pdf = scipy.stats.norm.logpdf(right_stats)
		r_cdf = log_ndtr(right_stats)
		right_frac = np.exp(r_pdf - r_cdf)
		beta_right = np.dot(right_frac, x_right / s)
		beta_jac += beta_right

		right_sigma = np.dot(right_frac, right_stats)
		sigma_jac -= right_sigma

	if y_mid is not None:
		mid_stats = (y_mid - np.dot(x_mid, b)) / s
		beta_mid = np.dot(mid_stats, x_mid / s)
		beta_jac += beta_mid

		mid_sigma = (np.square(mid_stats) - 1).sum() 
		sigma_jac += mid_sigma 
	
	combo_jac = np.append(beta_jac, sigma_jac / s)

	return - combo_jac 

class TobitModel:
	def __init__(self, fit_intercept=True):
		self.fit_intercept = fit_intercept 
		self.ols_coef_ = None 
		self.ols_intercept = None 
		self.coef_ = None
		self.intercept_ = None 
		self.sigma_ = None 

	def fit(self, x, y, cens, verbose=False):
		"""
		Fit maximum-likelihood Tobit regression
		cens: -1 indicates left-censored samples, 0 for uncensored, 1 for right-censored
		""" 
		x_copy = x.copy()
		if self.fit_intercept:
			x_copy.insert(0, 'intercept', 1.0)
		else:
			x_copy.scale(with_mean=True, with_std=False, copy=False)

		init_reg = LinearRegression(fit_intercept=False).fit(x_copy, y)
		b0 = init_reg.coef_ 
		y_pred = init_reg.predict(x_copy)
		resid = y - y_pred 
		resid_var = np.var(resid)
		s0 = np.sqrt(resid_var)
		params0 = np.append(b0, s0) 
		xs, ys = split_left_right_censored(x_copy, y, cens)

		result = minimize(lambda params: tobit_neg_log_likelihood(xs, ys, params), params0, method="BFGS",
							jac=lambda params: tobit_neg_log_likelihood_der(xs, ys, params), options={'disp': verbose})

		if verbose:
			print(result)
		self.ols_coef_ = b0[1:]
		self.ols_intercept = b0[0]
		if self.fit_intercept:
			self.intercept_ = result.x[1]
			self.coef_ = result.x[1:-1]
		else:
			self.intercept_ = 0
			self.coef_ = result.x[:-1]
		self.sigma_ = result.x[-1]

		return self 

	def predict(self, x):
		return self.intercept_ + np.dot(x, self.coef_)

	def score(self, x, y, scoring_function=mean_absolute_error):
		y_pred = self.predict(x)
		return scoring_function(y, y_pred)
		
```	