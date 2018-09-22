---
layout: post
title: SpringMVC Controller层统一处理异常的方法
date: 2018-09-22 09:38:12
categories: SpringMVC
tags: [SpringMVC]
---

# SpringMVC Controller层统一处理异常的方法

参考以下：

```
/**
 * 抽象的Controller层工具类，提供常见的异常处理机制。凡继承了此类的Controller，</br>
 * 在类名上添加注解"@ControllerAdvice"，就能够为该Controller中定义的所有带</br>
 * "@RequestMapping"注解的方法抛出的异常提供统一的处理方式。具体支持的异常处</br>
 * 理在此工具类中定义。
 * 
 * @author dev
 *
 */
public abstract class AbstractController extends ResponseEntityExceptionHandler {
	private static final Logger LOGGER = LoggerFactory.getLogger(TestCtrl.class);
	
	@Resource
	AuthField af;

	/**
	 * 处理异常类型为 JsonPaseException 的方法，</br>
	 * 在Controller层抛出的所有 JsonPaseException 类型的异常，</br>
	 * 都将进入此方法。在此方法中，将异常信息的按某种格式返回给前端页面，</br>
	 * 并且，打印详细的异常信息。</br>
	 * </br>
	 * 
	 * 返回前端的格式为: {"code":"500","message":"参数错误","exception":"具体的异常信息摘要"}
	 * 
	 * @param ex       异常对象
	 * @param request
	 * @param response
	 */
	@ResponseBody
	@ExceptionHandler(JsonPaseException.class)
	public void processJsonPaseException(JsonPaseException ex, HttpServletRequest request,
			HttpServletResponse response) {
		processException(ex, request, response);
	}
	
	@ResponseBody
	@ExceptionHandler(UnsupportedEncodingException.class)
	public void processUnsupportedEncodingException(UnsupportedEncodingException ex, HttpServletRequest request,
			HttpServletResponse response) {
		processException(ex, request, response);
	}

	@ResponseBody
	@ExceptionHandler(Exception.class)
	public void processException(Exception ex, HttpServletRequest request, HttpServletResponse response) {
		try {
			JSONObject json = ResultUtil.getResponse("500", "错误", ex.getMessage());
			response.setContentType("application/json");
			response.getWriter().printf(json.toJSONString());
			response.flushBuffer();
			LOGGER.error(ex.getMessage(), ex);
		} catch (IOException e) {
			LOGGER.error(e.getMessage(), e);
		}
	}
}
```
