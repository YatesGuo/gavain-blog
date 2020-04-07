---
title: HttpWebRequest的用法详解
date: 2019-12-12 14:06:51
tags: CSHARP
categories: Memo
---

1、HttpWebRequest和HttpWebResponse类是用于发送和接收HTTP数据的最好选择。
2、命名空间：System.Net
3、HttpWebRequest对象不是利用new关键字创建的（通过构造函数）。 
而是利用Create()方法创建的。
4、你可能预计需要显示地调用一个“Send”方法，实际上不需要。
5、调用 HttpWebRequest.GetResponse()方法返回的是一个HttpWebResponse对象
6、你可以把HTTP响应的数据流 （stream）绑定到一个StreamReader对象，然后就可以通过ReadToEnd()方法把整个HTTP响应作为一个字符串取回。也可以通过 StreamReader.ReadLine()方法逐行取回HTTP响应的内容。
下面是HttpWebRequest的一些属性，这些属性对于轻量级的自动化测试程序是非常重要的。
a） AllowAutoRedirect：获取或设置一个值，该值指示请求是否应跟随重定向响应。 
b）CookieContainer：获取或设置与此请求关联的cookie。 
c）Credentials：获取或设置请求的身份验证信息。 
d）KeepAlive：获取或设置一个值，该值指示是否与 Internet 资源建立持久性连接。 
e）MaximumAutomaticRedirections：获取或设置请求将跟随的重定向的最大数目。 
f） Proxy：获取或设置请求的代理信息。 
g）SendChunked：获取或设置一个值，该值指示是否将数据分段发送到 Internet 资源。 
h）Timeout：获取或设置请求的超时值。 
i） UserAgent：获取或设置 User-agent HTTP 标头的值
C# HttpWebRequest提交数据方式其实就是GET和POST两种
C# HttpWebRequest的作用： 
HttpWebRequest对HTTP协议进行了完整的封装，对HTTP协议中的 Header, Content, Cookie 都做了属性和方法的支持，很容易就能编写出一个模拟浏览器自动登录的程序。
C# HttpWebRequest提交数据方式： 
程序使用HTTP协议和服务器交互主要是进行数据的提交，通常数据的提交是通过 GET 和 POST 两种方式来完成，
C# HttpWebRequest提交数据方式
1. GET 方式。 
GET 方式通过在网络地址附加参数来完成数据的提交，比如在地址 http://www.google.com/webhp?hl=zh-CN 中，前面部分 http://www.google.com/webhp 表示数据提交的网址，后面部分 hl=zh-CN 表示附加的参数，其中 hl 表示一个键(key)， zh-CN 表示这个键对应的值(value)。程序代码如下：

		HttpWebRequest req =  
		(HttpWebRequest)HttpWebRequest.Create("http://www.google.com/webhp?hl=zh-CN" ); 
		req.Method = "GET"; 
		using (WebResponse wr = req.GetResponse()) 
		{ 
		   //在这里对接收到的页面内容进行处理 
		}

2. POST 方式。
POST 方式通过在页面内容中填写参数的方法来完成数据的提交，参数的格式和 GET 方式一样，是类似于 hl=zh-CN&newwindow=1 这样的结构。程序代码如下：

		string param = "hl=zh-CN&newwindow=1";        //参数
		byte[] bs = Encoding.ASCII.GetBytes(param);    //参数转化为ascii码
		HttpWebRequest req = (HttpWebRequest) HttpWebRequest.Create("http://www.google.com/intl/zh-CN/" );  //创建request
		req.Method = "POST";    //确定传值的方式，此处为post方式传值
		req.ContentType = "application/x-www-form-urlencoded"; 
		req.ContentLength = bs.Length; 
		using (Stream reqStream = req.GetRequestStream()) 
		{ 
		   reqStream.Write(bs, 0, bs.Length); 
		} 
		using (WebResponse wr = req.GetResponse()) 
		{ 
		   //在这里对接收到的页面内容进行处理 
		} 
		
3. 使用 GET 方式提交中文数据。
GET 方式通过在网络地址中附加参数来完成数据提交，对于中文的编码，常用的有 gb2312 和 utf8 两种，用 gb2312 方式编码访问的程序代码如下：
		Encoding myEncoding = Encoding.GetEncoding("gb2312");     //确定用哪种中文编码方式
		string address = "http://www.baidu.com/s?"+ HttpUtility.UrlEncode("参数一", myEncoding) +  "=" + HttpUtility.UrlEncode("值一", myEncoding);       //拼接数据提交的网址和经过中文编码后的中文参数
		HttpWebRequest req =   (HttpWebRequest)HttpWebRequest.Create(address);  //创建request
		req.Method = "GET";    //确定传值方式，此处为get方式
		using (WebResponse wr = req.GetResponse()) 
		{ 
		   //在这里对接收到的页面内容进行处理 
		} 
在上面的程序代码中，我们以 GET 方式访问了网址 http://www.baidu.com/s ，传递了参数“参数一=值一”，由于无法告知对方提交数据的编码类型，所以编码方式要以对方的网站为标准。常见的网站中， www.baidu.com （百度）的编码方式是 gb2312, www.google.com （谷歌）的编码方式是 utf8。
4. 使用 POST 方式提交中文数据。
POST 方式通过在页面内容中填写参数的方法来完成数据的提交，由于提交的参数中可以说明使用的编码方式，所以理论上能获得更大的兼容性。用 gb2312 方式编码访问的程序代码如下：

		Encoding myEncoding = Encoding.GetEncoding("gb2312");  //确定中文编码方式。此处用gb2312
		string param =   HttpUtility.UrlEncode("参数一", myEncoding) +   "=" + HttpUtility.UrlEncode("值一", myEncoding) +   "&" +     HttpUtility.UrlEncode("参数二", myEncoding) +  "=" + HttpUtility.UrlEncode("值二", myEncoding); 
		byte[] postBytes = Encoding.ASCII.GetBytes(param);     //将参数转化为assic码
		HttpWebRequest req = (HttpWebRequest)  HttpWebRequest.Create( "http://www.baidu.com/s" ); 
		req.Method = "POST"; 
		req.ContentType =   "application/x-www-form-urlencoded;charset=gb2312"; 
		req.ContentLength = postBytes.Length; 
		using (Stream reqStream = req.GetRequestStream()) 
		{ 
		   reqStream.Write(bs, 0, bs.Length); 
		} 
		using (WebResponse wr = req.GetResponse()) 
		{ 
		   //在这里对接收到的页面内容进行处理 
		}  

从上面的代码可以看出， POST 中文数据的时候，先使用 UrlEncode 方法将中文字符转换为编码后的 ASCII 码，然后提交到服务器，提交的时候可以说明编码的方式，用来使对方服务器能够正确的解析。
以上列出了客户端程序使用HTTP协议与服务器交互的情况，常用的是 GET 和 POST 方式。
现在流行的 WebService 也是通过 HTTP 协议来交互的，使用的是 POST 方法。与以上稍有所不同的是， WebService 提交的数据内容和接收到的数据内容都是使用了 XML 方式编码。所以， HttpWebRequest 也可以使用在调用 WebService 的情况下。



	#region 公共方法
        /// <summary>
        /// Get数据接口
        /// </summary>
        /// <param name="getUrl">接口地址</param>
        /// <returns></returns>
        private static string GetWebRequest(string getUrl)
        {
            string responseContent = "";
			HttpWebRequest request = (HttpWebRequest)WebRequest.Create(getUrl);
            request.ContentType = "application/json";
            request.Method = "GET";
			HttpWebResponse response = (HttpWebResponse)request.GetResponse();
            //在这里对接收到的页面内容进行处理
            using (Stream resStream = response.GetResponseStream())
            {
                using (StreamReader reader = new StreamReader(resStream, Encoding.UTF8))
                {
                    responseContent = reader.ReadToEnd().ToString();
                }
            }
            return responseContent;
        }
        /// <summary>
        /// Post数据接口
        /// </summary>
        /// <param name="postUrl">接口地址</param>
        /// <param name="paramData">提交json数据</param>
        /// <param name="dataEncode">编码方式(Encoding.UTF8)</param>
        /// <returns></returns>
        private static string PostWebRequest(string postUrl, string paramData, Encoding dataEncode)
        {
            string responseContent = string.Empty;
            try
            {
                byte[] byteArray = dataEncode.GetBytes(paramData); //转化
                HttpWebRequest webReq = (HttpWebRequest)WebRequest.Create(new Uri(postUrl));
                webReq.Method = "POST";
                webReq.ContentType = "application/x-www-form-urlencoded";
                webReq.ContentLength = byteArray.Length;
                using (Stream reqStream = webReq.GetRequestStream())
                {
                    reqStream.Write(byteArray, 0, byteArray.Length);//写入参数
                    //reqStream.Close();
                }
                using (HttpWebResponse response = (HttpWebResponse)webReq.GetResponse())
                {
                    //在这里对接收到的页面内容进行处理
                    using (StreamReader sr = new StreamReader(response.GetResponseStream(), Encoding.Default))
                    {
                        responseContent = sr.ReadToEnd().ToString();
                    }
                }
            }
            catch (Exception ex)
            {
                return ex.Message;
            }
            return responseContent;
        }
	#endregion

post支持cookie


        /// <summary>
        /// 发起Post请求（采用HttpWebRequest，支持传Cookie）
        /// </summary>
        /// <param name="strUrl">请求Url</param>
        /// <param name="formData">发送的表单数据</param>
        /// <param name="strResult">返回请求结果（如果请求失败，返回异常消息）</param>
        /// <param name="cookieContainer">随同HTTP请求发送的Cookie信息，如果不需要身份验证可以为空</param>
        /// <returns>返回：是否请求成功</returns>
        public static bool HttpPost(string strUrl, Dictionary<string, string> formData, CookieContainer cookieContainer, out string strResult)
        {
            string strPostData = null;
            if (formData != null && formData.Count > 0)
            {
                StringBuilder sbData = new StringBuilder();
                int i = 0;
                foreach (KeyValuePair<string, string> data in formData)
                {
                    if (i > 0)
                    {
                        sbData.Append("&");
                    }
                    sbData.AppendFormat("{0}={1}", data.Key, data.Value);
                    i++;
                }
                strPostData = sbData.ToString();
            }
		byte[] postBytes = string.IsNullOrEmpty(strPostData) ? new byte[0] : Encoding.UTF8.GetBytes(strPostData);
		return HttpPost(strUrl, postBytes, cookieContainer, out strResult);
				}

		post上传文件



        /// <summary>
        /// 发起Post文件请求（采用HttpWebRequest，支持传Cookie）
        /// </summary>
        /// <param name="strUrl">请求Url</param>
        /// <param name="strFilePostName">上传文件的PostName</param>
        /// <param name="strFilePath">上传文件路径</param>
        /// <param name="cookieContainer">随同HTTP请求发送的Cookie信息，如果不需要身份验证可以为空</param>
        /// <param name="strResult">返回请求结果（如果请求失败，返回异常消息）</param>
        /// <returns>返回：是否请求成功</returns>
        public static bool HttpPostFile(string strUrl, string strFilePostName, string strFilePath, CookieContainer cookieContainer, out string strResult)
        {
            HttpWebRequest request = null;
            FileStream fileStream = FileHelper.GetFileStream(strFilePath);
			try
            {
                if (fileStream == null)
                {
                    throw new FileNotFoundException();
                }
				request = (HttpWebRequest)WebRequest.Create(strUrl);
                request.Method = "POST";
                request.KeepAlive = false;
                request.Timeout = 30000;
				if (cookieContainer != null)
                {
                    request.CookieContainer = cookieContainer;
                }
				string boundary = string.Format("---------------------------{0}", DateTime.Now.Ticks.ToString("x"));
				byte[] header = Encoding.UTF8.GetBytes(string.Format("--{0}\r\nContent-Disposition: form-data; name=\"{1}\"; filename=\"{2}\"\r\nContent-Type: application/octet-stream\r\n\r\n",
                    boundary, strFilePostName, Path.GetFileName(strFilePath)));
                byte[] footer = Encoding.UTF8.GetBytes(string.Format("\r\n--{0}--\r\n", boundary));
				request.ContentType = string.Format("multipart/form-data; boundary={0}", boundary);
                request.ContentLength = header.Length + fileStream.Length + footer.Length;
				using (Stream reqStream = request.GetRequestStream())
                {
                    // 写入分割线及数据信息
                    reqStream.Write(header, 0, header.Length);
					// 写入文件
                    FileHelper.WriteFile(reqStream, fileStream);
					// 写入尾部
                    reqStream.Write(footer, 0, footer.Length);
                }
				strResult = GetResponseResult(request, cookieContainer);
            }
            catch (Exception ex)
            {
                strResult = ex.Message;
                return false;
            }
            finally
            {
                if (request != null)
                {
                    request.Abort();
                }
                if (fileStream != null)
                {
                    fileStream.Close();
                }
            }
			return true;
        }
		/// <summary>
        /// 获取请求结果字符串
        /// </summary>
        /// <param name="request">请求对象（发起请求之后）</param>
        /// <param name="cookieContainer">随同HTTP请求发送的Cookie信息，如果不需要身份验证可以为空</param>
        /// <returns>返回请求结果字符串</returns>
        private static string GetResponseResult(HttpWebRequest request, CookieContainer cookieContainer = null)
        {
            using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
            {
                if (cookieContainer != null)
                {
                    response.Cookies = cookieContainer.GetCookies(response.ResponseUri);
                }
                using (Stream rspStream = response.GetResponseStream())
                {
                    using (StreamReader reader = new StreamReader(rspStream, Encoding.UTF8))
                    {
                        return reader.ReadToEnd();
                    }
                }
            }
        }

 
OAuth头部 

		//构造OAuth头部 
		StringBuilder oauthHeader = new StringBuilder(); 
		oauthHeader.AppendFormat("OAuth realm=\"\", oauth_consumer_key={0}, ", apiKey); 
		oauthHeader.AppendFormat("oauth_nonce={0}, ", nonce); 
		oauthHeader.AppendFormat("oauth_timestamp={0}, ", timeStamp); 
		oauthHeader.AppendFormat("oauth_signature_method={0}, ", "HMAC-SHA1"); 
		oauthHeader.AppendFormat("oauth_version={0}, ", "1.0"); 
		oauthHeader.AppendFormat("oauth_signature={0}, ", sig); 
		oauthHeader.AppendFormat("oauth_token={0}", accessToken); 
		//构造请求 
		StringBuilder requestBody = new StringBuilder(""); 
		Encoding encoding = Encoding.GetEncoding("utf-8"); 
		byte[] data = encoding.GetBytes(requestBody.ToString()); 
		// Http Request的设置 
		HttpWebRequest request = (HttpWebRequest)WebRequest.Create(uri); 
		request.Headers.Set("Authorization", oauthHeader.ToString());
		//request.Headers.Add("Authorization", authorization); 
		request.ContentType = "application/atom+xml"; 
		request.Method = "GET"; 

 
C#通过WebClient/HttpWebRequest实现http的post/get方法
1.POST方法(httpWebRequest)
 

		//body是要传递的参数,格式"roleId=1&uid=2"
		//post的cotentType填写:"application/x-www-form-urlencoded"
		//soap填写:"text/xml; charset=utf-8"
		public static string PostHttp(string url, string body, string contentType)
		{
			HttpWebRequest httpWebRequest = (HttpWebRequest)WebRequest.Create(url);
			httpWebRequest.ContentType = contentType;
			httpWebRequest.Method = "POST";
			httpWebRequest.Timeout = 20000;
			byte[] btBodys = Encoding.UTF8.GetBytes(body);
			httpWebRequest.ContentLength = btBodys.Length;
			httpWebRequest.GetRequestStream().Write(btBodys, 0, btBodys.Length);
			HttpWebResponse httpWebResponse = (HttpWebResponse)httpWebRequest.GetResponse();
			StreamReader streamReader = new StreamReader(httpWebResponse.GetResponseStream());
			string responseContent = streamReader.ReadToEnd();
			httpWebResponse.Close();
			streamReader.Close();
			httpWebRequest.Abort();
			httpWebResponse.Close();
			return responseContent;
		}
POST方法(httpWebRequest)


2.POST方法(WebClient)

		/// <summary>
        /// 通过WebClient类Post数据到远程地址，需要Basic认证；
        /// 调用端自己处理异常
        /// </summary>
        /// <param name="uri"></param>
        /// <param name="paramStr">name=张三&age=20</param>
        /// <param name="encoding">请先确认目标网页的编码方式</param>
        /// <param name="username"></param>
        /// <param name="password"></param>
        /// <returns></returns>
        public static string Request_WebClient(string uri, string paramStr, Encoding encoding, string username, string password)
        {
            if (encoding == null)
                encoding = Encoding.UTF8;
			string result = string.Empty;
			WebClient wc = new WebClient();
			// 采取POST方式必须加的Header
            wc.Headers.Add("Content-Type", "application/x-www-form-urlencoded");
			byte[] postData = encoding.GetBytes(paramStr);
			if (!string.IsNullOrEmpty(username) && !string.IsNullOrEmpty(password))
            {
                wc.Credentials = GetCredentialCache(uri, username, password);
                wc.Headers.Add("Authorization", GetAuthorization(username, password));
            }
			byte[] responseData = wc.UploadData(uri, "POST", postData); // 得到返回字符流
            return encoding.GetString(responseData);// 解码                  
        }
POST方法(WebClient)

3.Get方法(httpWebRequest)

	public static string GetHttp(string url, HttpContext httpContext)
	{
		string queryString = "?";
		foreach (string key in httpContext.Request.QueryString.AllKeys)
		{
			queryString += key + "=" + httpContext.Request.QueryString[key] + "&";
		}
		queryString = queryString.Substring(0, queryString.Length - 1);
		HttpWebRequest httpWebRequest = (HttpWebRequest)WebRequest.Create(url + queryString);
		httpWebRequest.ContentType = "application/json";
		httpWebRequest.Method = "GET";
		httpWebRequest.Timeout = 20000;
		//byte[] btBodys = Encoding.UTF8.GetBytes(body);
		//httpWebRequest.ContentLength = btBodys.Length;
		//httpWebRequest.GetRequestStream().Write(btBodys, 0, btBodys.Length);
		HttpWebResponse httpWebResponse = (HttpWebResponse)httpWebRequest.GetResponse();
		StreamReader streamReader = new StreamReader(httpWebResponse.GetResponseStream());
		string responseContent = streamReader.ReadToEnd();
		httpWebResponse.Close();
		streamReader.Close();
		return responseContent;
	}
Get方法(HttpWebRequest)

4.basic验证的WebRequest/WebResponse

	/// <summary>
    /// 通过 WebRequest/WebResponse 类访问远程地址并返回结果，需要Basic认证；
    /// 调用端自己处理异常
    /// </summary>
    /// <param name="uri"></param>
    /// <param name="timeout">访问超时时间，单位毫秒；如果不设置超时时间，传入0</param>
    /// <param name="encoding">如果不知道具体的编码，传入null</param>
    /// <param name="username"></param>
    /// <param name="password"></param>
    /// <returns></returns>
    public static string Request_WebRequest(string uri, int timeout, Encoding encoding, string username, string password)
    {
        string result = string.Empty;
		WebRequest request = WebRequest.Create(new Uri(uri));
		if (!string.IsNullOrEmpty(username) && !string.IsNullOrEmpty(password))
        {
            request.Credentials = GetCredentialCache(uri, username, password);
            request.Headers.Add("Authorization", GetAuthorization(username, password));
        }
		if (timeout > 0)
            request.Timeout = timeout;
		WebResponse response = request.GetResponse();
        Stream stream = response.GetResponseStream();
        StreamReader sr = encoding == null ? new StreamReader(stream) : new StreamReader(stream, encoding);
		result = sr.ReadToEnd();
		sr.Close();
        stream.Close();
		return result;
    }
	
	#region # 生成 Http Basic 访问凭证 #
		private static CredentialCache GetCredentialCache(string uri, string username, string password)
		{
			string authorization = string.Format("{0}:{1}", username, password);
			CredentialCache credCache = new CredentialCache();
			credCache.Add(new Uri(uri), "Basic", new NetworkCredential(username, password));
			return credCache;
		}
		private static string GetAuthorization(string username, string password)
		{
			string authorization = string.Format("{0}:{1}", username, password);
			return "Basic " + Convert.ToBase64String(new ASCIIEncoding().GetBytes(authorization));
		}
	#endregion
	
basic验证的WebRequest/WebResponse

 
C#语言写的关于HttpWebRequest 类的使用方法
http://www.jb51.net/article/57156.htm

	using System;
	using System.Collections.Generic;
	using System.IO;
	using System.Net;
	using System.Text;
	namespace HttpWeb
	{
	    /// <summary> 
	    ///  Http操作类 
	    /// </summary> 
	    public static class httptest
	    {
	        /// <summary> 
	        ///  获取网址HTML 
	        /// </summary> 
	        /// <param name="URL">网址 </param> 
	        /// <returns> </returns> 
	        public static string GetHtml(string URL)
	        {
	            WebRequest wrt;
	            wrt = WebRequest.Create(URL);
	            wrt.Credentials = CredentialCache.DefaultCredentials;
	            WebResponse wrp;
	            wrp = wrt.GetResponse();
	            string reader = new StreamReader(wrp.GetResponseStream(), Encoding.GetEncoding("gb2312")).ReadToEnd();
	            try
	            {
	                wrt.GetResponse().Close();
	            }
	            catch (WebException ex)
	            {
	                throw ex;
	            }
	            return reader;
	        }
	        /// <summary> 
	        /// 获取网站cookie 
	        /// </summary> 
	        /// <param name="URL">网址 </param> 
	        /// <param name="cookie">cookie </param> 
	        /// <returns> </returns> 
	        public static string GetHtml(string URL, out string cookie)
	        {
	            WebRequest wrt;
	            wrt = WebRequest.Create(URL);
	            wrt.Credentials = CredentialCache.DefaultCredentials;
	            WebResponse wrp;
	            wrp = wrt.GetResponse();
	            string html = new StreamReader(wrp.GetResponseStream(), Encoding.GetEncoding("gb2312")).ReadToEnd();
	            try
	            {
	                wrt.GetResponse().Close();
	            }
	            catch (WebException ex)
	            {
	                throw ex;
	            }
	            cookie = wrp.Headers.Get("Set-Cookie");
	            return html;
	        }
	        public static string GetHtml(string URL, string postData, string cookie, out string header, string server)
	        {
	            return GetHtml(server, URL, postData, cookie, out header);
	        }
	        public static string GetHtml(string server, string URL, string postData, string cookie, out string header)
	        {
	            byte[] byteRequest = Encoding.GetEncoding("gb2312").GetBytes(postData);
	            return GetHtml(server, URL, byteRequest, cookie, out header);
	        }
	        public static string GetHtml(string server, string URL, byte[] byteRequest, string cookie, out string header)
	        {
	            byte[] bytes = GetHtmlByBytes(server, URL, byteRequest, cookie, out header);
	            Stream getStream = new MemoryStream(bytes);
	            StreamReader streamReader = new StreamReader(getStream, Encoding.GetEncoding("gb2312"));
	            string getString = streamReader.ReadToEnd();
	            streamReader.Close();
	            getStream.Close();
	            return getString;
	        }
	        /// <summary> 
	        /// Post模式浏览 
	        /// </summary> 
	        /// <param name="server">服务器地址 </param> 
	        /// <param name="URL">网址 </param> 
	        /// <param name="byteRequest">流 </param> 
	        /// <param name="cookie">cookie </param> 
	        /// <param name="header">句柄 </param> 
	        /// <returns> </returns> 
	        public static byte[] GetHtmlByBytes(string server, string URL, byte[] byteRequest, string cookie, out string header)
	        {
	            long contentLength;
	            HttpWebRequest httpWebRequest;
	            HttpWebResponse webResponse;
	            Stream getStream;
	            httpWebRequest = (HttpWebRequest)HttpWebRequest.Create(URL);
	            CookieContainer co = new CookieContainer();
	            co.SetCookies(new Uri(server), cookie);
	            httpWebRequest.CookieContainer = co;
	            httpWebRequest.ContentType = "application/x-www-form-urlencoded";
	            httpWebRequest.Accept =
	                "image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/x-shockwave-flash, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, */*";
	            httpWebRequest.Referer = server;
	            httpWebRequest.UserAgent =
	                "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; Maxthon; .NET CLR 1.1.4322)";
	            httpWebRequest.Method = "Post";
	            httpWebRequest.ContentLength = byteRequest.Length;
	            Stream stream;
	            stream = httpWebRequest.GetRequestStream();
	            stream.Write(byteRequest, 0, byteRequest.Length);
	            stream.Close();
	            webResponse = (HttpWebResponse)httpWebRequest.GetResponse();
	            header = webResponse.Headers.ToString();
	            getStream = webResponse.GetResponseStream();
	            contentLength = webResponse.ContentLength;
	            byte[] outBytes = new byte[contentLength];
	            outBytes = ReadFully(getStream);
	            getStream.Close();
	            return outBytes;
	        }
	        public static byte[] ReadFully(Stream stream)
	        {
	            byte[] buffer = new byte[128];
	            using (MemoryStream ms = new MemoryStream())
	            {
	                while (true)
	                {
	                    int read = stream.Read(buffer, 0, buffer.Length);
	                    if (read <= 0)
	                        return ms.ToArray();
	                    ms.Write(buffer, 0, read);
	                }
	            }
	        }
	        /// <summary> 
	        /// Get模式 
	        /// </summary> 
	        /// <param name="URL">网址 </param> 
	        /// <param name="cookie">cookies </param> 
	        /// <param name="header">句柄 </param> 
	        /// <param name="server">服务器 </param> 
	        /// <param name="val">服务器 </param> 
	        /// <returns> </returns> 
	        public static string GetHtml(string URL, string cookie, out string header, string server)
	        {
	            return GetHtml(URL, cookie, out header, server, "");
	        }
	        /// <summary> 
	        /// Get模式浏览 
	        /// </summary> 
	        /// <param name="URL">Get网址 </param> 
	        /// <param name="cookie">cookie </param> 
	        /// <param name="header">句柄 </param> 
	        /// <param name="server">服务器地址 </param> 
	        /// <param name="val"> </param> 
	        /// <returns> </returns> 
	        public static string GetHtml(string URL, string cookie, out string header, string server, string val)
	        {
	            HttpWebRequest httpWebRequest;
	            HttpWebResponse webResponse;
	            Stream getStream;
	            StreamReader streamReader;
	            string getString = "";
	            httpWebRequest = (HttpWebRequest)HttpWebRequest.Create(URL);
	            httpWebRequest.Accept = "*/*";
	            httpWebRequest.Referer = server;
	            CookieContainer co = new CookieContainer();
	            co.SetCookies(new Uri(server), cookie);
	            httpWebRequest.CookieContainer = co;
	            httpWebRequest.UserAgent =
	                "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; Maxthon; .NET CLR 1.1.4322)";
	            httpWebRequest.Method = "GET";
	            webResponse = (HttpWebResponse)httpWebRequest.GetResponse();
	            header = webResponse.Headers.ToString();
	            getStream = webResponse.GetResponseStream();
	            streamReader = new StreamReader(getStream, Encoding.GetEncoding("gb2312"));
	            getString = streamReader.ReadToEnd();
	            streamReader.Close();
	            getStream.Close();
	            return getString;
	        }
		}
	}

 有空查看：



    /// <summary>
    /// 作用描述： WebRequest操作类
    /// </summary>
    public static class CallWebRequest
    {
        /// <summary>
        /// 通过GET请求获取数据
        /// </summary>
        /// <param name="url"></param>
        /// <returns></returns>
        public static string Get(string url)
        {
            HttpWebRequest request = WebRequest.Create(url) as HttpWebRequest;
            //每次请求绕过代理，解决第一次调用慢的问题
            request.Proxy = null;
            //多线程并发调用时默认2个http连接数限制的问题，讲其设为1000
            ServicePoint currentServicePoint = request.ServicePoint;
            currentServicePoint.ConnectionLimit = 1000;
            HttpWebResponse response = request.GetResponse() as HttpWebResponse;
            string str;
            using (Stream responseStream = response.GetResponseStream())
            {
                Encoding encode = Encoding.GetEncoding("utf-8");
                StreamReader reader = new StreamReader(responseStream, encode);
                str = reader.ReadToEnd();          
            }
            response.Close();
            response = null;
            request.Abort();
            request = null;
            return str;
        }
		
		/// <summary>
        /// 通过POST请求传递数据
        /// </summary>
        /// <param name="url"></param>
        /// <param name="postData"></param>
        /// <returns></returns>
        public static string Post(string url, string postData)
        {
            HttpWebRequest request = WebRequest.Create(url) as HttpWebRequest;
			//将发送数据转换为二进制格式
            byte[] byteArray = Encoding.UTF8.GetBytes(postData);
            //要POST的数据大于1024字节的时候, 浏览器并不会直接就发起POST请求, 而是会分为俩步：
            //1. 发送一个请求, 包含一个Expect:100-continue, 询问Server使用愿意接受数据
            //2. 接收到Server返回的100-continue应答以后, 才把数据POST给Server
            //直接关闭第一步验证
            request.ServicePoint.Expect100Continue = false;
            //是否使用Nagle：不使用，提高效率 
            request.ServicePoint.UseNagleAlgorithm = false;
            //设置最大连接数
            request.ServicePoint.ConnectionLimit = 65500;
            //指定压缩方法
            //request.Headers.Add(HttpRequestHeader.AcceptEncoding, "gzip,deflate");
            request.KeepAlive = false;
            //request.ContentType = "application/json";
            request.ContentType = "application/x-www-form-urlencoded";
            
            request.Method = "POST";
            request.Timeout = 20000;
            request.ContentLength = byteArray.Length;
            //关闭缓存
            request.AllowWriteStreamBuffering = false;
			//每次请求绕过代理，解决第一次调用慢的问题
            request.Proxy = null;
            //多线程并发调用时默认2个http连接数限制的问题，讲其设为1000
            ServicePoint currentServicePoint = request.ServicePoint;
			Stream dataStream = request.GetRequestStream();
            dataStream.Write(byteArray, 0, byteArray.Length);
            dataStream.Close();
			WebResponse response = request.GetResponse();
            //获取服务器返回的数据流
            dataStream = response.GetResponseStream();
            StreamReader reader = new StreamReader(dataStream);
            string responseFromServer = reader.ReadToEnd();
            reader.Close();
            dataStream.Close();
            request.Abort();
            response.Close();
            reader.Dispose();
            dataStream.Dispose();
            return responseFromServer;
        }
    }

 



    /// <summary>
    /// 用于外部接口调用封装
    /// </summary>
    public static class ApiInterface
    {
		/// <summary>
        ///验证密码是否正确
        /// </summary>
        public static Api_ToConfig<object> checkPwd(int userId, string old)
        {
            var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/checkPwd";
            var data = Json.ToObject<Api_ToConfig<object>>(CallWebRequest.Post(url + "?id=" + userId, "&oldPwd=" + old));
            //var data = Json.ToObject<Api_ToConfig<object>>(CallWebRequest.Post(url, "{\"id\":" + userId + ",oldPwd:\"" + old + "\",newPwd:\"" + pwd + "\"}"));
            return data;
        }
		/// <summary>
        ///
        /// </summary>
        public static Api_ToConfig<object> UpdatePassword(int userId, string pwd, string old)
        {
            var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/updatePwd";
            var data = Json.ToObject<Api_ToConfig<object>>(CallWebRequest.Post(url + "?id=" + userId + "&oldPwd=" + old, "&newPwd=" + pwd));
            //var data = Json.ToObject<Api_ToConfig<object>>(CallWebRequest.Post(url, "{\"id\":" + userId + ",oldPwd:\"" + old + "\",newPwd:\"" + pwd + "\"}"));
            return data;
        }
	private static DateTime GetTime(string timeStamp)
        {
            DateTime dtStart = TimeZone.CurrentTimeZone.ToLocalTime(new DateTime(1970, 1, 1));
            long lTime = long.Parse(timeStamp + "0000");
            TimeSpan toNow = new TimeSpan(lTime); return dtStart.Add(toNow);
        }
	#region 公共方法
		/// <summary>
        /// 配置文件key
        /// </summary>
        /// <param name="key"></param>
        /// <param name="isOutKey"></param>
        /// <returns></returns>
        public static string GetConfig(string key, bool isOutKey = false)
        {
            //  CacheRedis.Cache.RemoveObject(RedisKey.ConfigList);
            var data = CacheRedis.Cache.Get<Api_BaseToConfig<Api_Config>>(RedisKey.ConfigList);
			//  var data = new Api_BaseToConfig<Api_Config>();
            if (data == null)
            {
                string dataCenterUrl = WebConfigManager.GetAppSettings("DataCenterUrl");
                string configurationInfoListByFilter = WebConfigManager.GetAppSettings("ConfigurationInfoListByFilter");
				string systemCoding = WebConfigManager.GetAppSettings("SystemCoding");
                string nodeIdentifier = WebConfigManager.GetAppSettings("NodeIdentifier");
                string para = "systemIdf=" + systemCoding + "&nodeIdf=" + nodeIdentifier + "";
                //string para = "{\"systemIdf\":\"" + systemCoding + "\",\"nodeIdf\":\"" + nodeIdentifier + "\"}";
				string result = CallWebRequest.Post(dataCenterUrl + "/rest" + configurationInfoListByFilter, para);
                data =
                    Json.ToObject<Api_BaseToConfig<Api_Config>>(result);
                CacheRedis.Cache.Set(RedisKey.ConfigList, data);
            }
            if (data.Status)
            {
                if (isOutKey && ConfigServer.IsOutside())
                {
                    key += "_outside";
                }
                key = key.ToLower();
                var firstOrDefault = data.Data.FirstOrDefault(e => e.Identifier.ToLower() == key);
                if (firstOrDefault != null)
                {
                    return firstOrDefault.Value;
                }
                else
                {
                    if (key.IndexOf("_outside") > -1)
                    {
                        firstOrDefault = data.Data.FirstOrDefault(e => e.Identifier == key.Substring(0, key.LastIndexOf("_outside")));
                        if (firstOrDefault != null)
                            return firstOrDefault.Value;
                    }
                }
            }
            return "";
        }
	public static string WebPostRequest(string url, string postData)
			{
				return CallWebRequest.Post(url, postData);
			}
			public static string WebGetRequest(string url)
			{
				return CallWebRequest.Get(url);
			}
	#endregion
	#region 参数转换方法
			private static string ConvertClassIds(string classIds)
			{
				var list = classIds.Split(',');
				StringBuilder sb = new StringBuilder("{\"class_id_list\": [");
				foreach (var s in list)
				{
					sb.Append("{\"classId\": \"" + s + "\"},");
				}
				if (list.Any())
					sb.Remove(sb.Length - 1, 1);
	sb.Append("]}");
	return sb.ToString();
			}
	private static string ConvertLabIds(string labIds)
			{
				var list = labIds.Split(',');
				StringBuilder sb = new StringBuilder("{\"lab_id_list\": [");
				foreach (var s in list)
				{
					sb.Append("{\"labId\": \"" + s + "\"},");
				}
				if (list.Any())
					sb.Remove(sb.Length - 1, 1);
	sb.Append("]}");
	return sb.ToString();
			}
	private static string ConvertCardNos(string cardNos)
			{
				var list = cardNos.Split(',');
				StringBuilder sb = new StringBuilder("{\"student_cardNos_list\": [");
				foreach (var s in list)
				{
					sb.Append("{\"stuCardNo\": \"" + s + "\"},");
				}
				if (list.Any())
					sb.Remove(sb.Length - 1, 1);
	sb.Append("]}");
	return sb.ToString();
			}
			#endregion
	/// <summary>
			/// 设置公文已读
			/// </summary>
			/// <param name="beginTime"></param>
			/// <param name="endTime"></param>
			/// <param name="userId"></param>
			/// <param name="currentPage"></param>
			/// <param name="pageSize"></param>
			/// <returns></returns>
			public static Api_ToConfig<object> FlowService(string id)
			{
				var url = WebConfigManager.GetAppSettings("FlowService");
				var data = Json.ToObject<Api_ToConfig<object>>(CallWebRequest.Post(url + "?Copyid=" + id, ""));
				return data;
			}
	/// <summary>
			///获取OA工作流数据
			/// </summary>
			public static string getOAWorkFlowData(string userName, string userId)
			{
				var url = GetConfig("platform.application.oa.url") + WebConfigManager.GetAppSettings("OAWorkFlowData");
				var data = CallWebRequest.Post(url, "&account=" + userName + "&userId=" + userId);
				return data;
	}
	public static List<Api_Course> GetDreamClassCourse(string userId)
			{
				List<Api_Course> list = new List<Api_Course>();
				list = CacheRedis.Cache.Get<List<Api_Course>>(RedisKey.DreamCourse + userId);
				if (list != null && list.Count > 0)
					return list;
	var url = GetConfig("platform.system.dreamclass.url", true) + "rest/getTeacherCourseInfo";
				var result = Json.ToObject<ApiResult<Api_Course>>(CallWebRequest.Post(url, "&userId=" + userId));
				if (result.state)
					list = result.data;
				CacheRedis.Cache.Set(RedisKey.DreamCourse + userId, list, 30);//缓存30分钟失效
				return list;
			}
	/// <summary>
			/// 用户信息
			/// </summary>
			/// <param name="userId"></param>
			/// <returns></returns>
			public static Api_User GetUserInfoByUserName(string userName)
			{
				var model = new Api_User();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/getUserInfoByUserName";
				var result = Json.ToObject<ApiBase<Api_User>>(CallWebRequest.Post(url, "&userName=" + userName));
				if (result.status == true && result.data != null)
					model = result.data;
				return model;
			}
	public static Api_User GetUserInfoByUserId(string userId)
			{
				var model = new Api_User();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/getUserInfoByUserId";
				var result = Json.ToObject<ApiBase<Api_User>>(CallWebRequest.Get(url + "?userId=" + userId));
				if (result.status == true && result.data != null)
					model = result.data;
				return model;
			}
	public static Api_User GetUserByUserId(string userId)
			{
				var model = new Api_User();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/getUserByUserId";
				var result = Json.ToObject<ApiBase<Api_User>>(CallWebRequest.Get(url + "?userId=" + userId));
				if (result.status == true && result.data != null)
					model = result.data;
				return model;
			}
	public static ApiBase<string> UserExist(string userName)
			{
				ApiBase<string> result = new ApiBase<string>();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/checkUserExists";
				result = Json.ToObject<ApiBase<string>>(CallWebRequest.Post(url, "&userName=" + userName));
				return result;
	}
			public static ApiBase<string> CheckUserPwd(string userName, string pwd)
			{
				ApiBase<string> result = new ApiBase<string>();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/checkUserPsw";
				result = Json.ToObject<ApiBase<string>>(CallWebRequest.Post(url, "&userName=" + userName + "&psw=" + pwd));
				return result;
	}
			public static ApiBase<Api_AnswerQuestion> GetAnswerQuestion(string userName)
			{
				ApiBase<Api_AnswerQuestion> result = new ApiBase<Api_AnswerQuestion>();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/safety/getSafetyByUserName";
				result = Json.ToObject<ApiBase<Api_AnswerQuestion>>(CallWebRequest.Post(url, "&userName=" + userName));
				return result;
			}
	public static ApiBase<string> ResetUserPassword(string userName, string newPassWord)
			{
				ApiBase<string> result = new ApiBase<string>();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/resetUserPasswordByUserName";
				result = Json.ToObject<ApiBase<string>>(CallWebRequest.Post(url, "&userName=" + userName + "&newPassWord=" + newPassWord));
				return result;
			}
			public static ApiBase<string> ChangeUserSafeAnswer(string userId, string answer1, string answer2, string answer3, string safeId)
			{
				ApiBase<string> result = new ApiBase<string>();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/safety/batchUpdageSafety";
				result = Json.ToObject<ApiBase<string>>(CallWebRequest.Post(url, "&data=[{editor:\"" + userId + "\",safetyAnswerOne:\"" + answer1 + "\",safetyAnswerTwo:\"" + answer2 + "\",safetyAnswerThree:\"" + answer3 + "\",safetyId:" + safeId + "}]"));
				return result;
			}
	public static ApiBase<string> UpdateUserPhoto(string userId, string userPhoto)
			{
				ApiBase<string> result = new ApiBase<string>();
				var url = WebConfigManager.GetAppSettings("DataCenterUrl") + "/rest/user/updataUserInfo";
				result = Json.ToObject<ApiBase<string>>(CallWebRequest.Post(url, "&data=[{userId:\"" + userId + "\",userPhoto:\"" + userPhoto + "\"}]"));
				return result;
			}
	public static Api_DreamClassUserInfo GetDreamClassUser(string userId)
			{
				var model = new Api_DreamClassUserInfo();
				//model = CacheRedis.Cache.Get<Api_DreamClassUserInfo>(RedisKey.DreamCourseUser + userId);
				var url = GetConfig("platform.system.dreamclass.url", true) + "rest/getUserInfo";
				var result = Json.ToObject<ApiBase<Api_DreamClassUserInfo>>(CallWebRequest.Post(url, "&userId=" + userId));
				if (result.state == true && result.data != null)
				{
					model = result.data;
					//CacheRedis.Cache.Set(RedisKey.DreamCourseUser+userId,model);
				}
	return model;
			}
	public static List<Api_BitCourse> GetBitCourseList(string userName)
			{
				List<Api_BitCourse> list = new List<Api_BitCourse>();
				list = CacheRedis.Cache.Get<List<Api_BitCourse>>(RedisKey.BitCourse + userName);
				if (list != null && list.Count > 0)
					return list;
				try
				{
					var url = GetConfig("platform.system.szhjxpt.url", true) + "/Services/DtpServiceWJL.asmx/GetMyCourseByLoginName";
					var result = Json.ToObject<ApiConfig<Api_BitCourse>>(CallWebRequest.Post(url, "&loginName=" + userName));
	if (result.success)
						list = result.datas;
					CacheRedis.Cache.Set(RedisKey.BitCourse + userName, list, 30);//缓存30分钟失效
				}
				catch (Exception exception)
				{
					list = new List<Api_BitCourse>();
					Log.Error(exception.Message);
				}
	return list;
			}
		}

