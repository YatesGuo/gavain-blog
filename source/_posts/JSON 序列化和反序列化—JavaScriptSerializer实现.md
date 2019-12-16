---
title: JSON 序列化和反序列化—JavaScriptSerializer实现
date: 2018-8-25
tag: CSHARP
---
JavaScriptSerializer 类由异步通信层内部使用，用于序列化和反序列化在浏览器和 Web 服务器之间传递的数据。您无法访问序列化程序的此实例。但是，此类公开了公共 API。因此，当您希望在托管代码中使用 JavaScript 对象符号 (JSON) 时可以使用此类。
若要序列化对象，请使用 Serialize 方法。若要反序列化 JSON 字符串，请使用 Deserialize 或 DeserializeObject 方法。若要序列化和反序列化 JavaScriptSerializer 本身不支持的类型，请使用 JavaScriptConverter 类来实现自定义转换器。然后，使用 RegisterConverters 方法注册转换器。
将 JSON 字符串反序列化为托管类型时，采用相同的映射。但是，反序列化可能是非对称的，并非所有可序列化的托管类型都可以从 JSON 反序列化得到。
通过JavaScriptSerializer来实现。它的名字空间为：System.Web.Script.Serialization
如果要使用它，还须添加 System.Web.Extensions 库文件引用
参考实体类：Customer

		public class Customer
		{
			public int Unid { get; set; }
			public string CustomerName { get; set; }
		}
	
类 JavaScriptSerializer 描述：为启用 AFAX 的应用程序提供序列化和反序列化功能。
（一） 序列化
方法：public string Serialize(Object obj)，用于将对象转换为 JSON 字符串

		public string ScriptSerialize(Customer customer)
		{
			JavaScriptSerializer js = new JavaScriptSerializer();
			return js.Serialize(customer);
		}
	
测试：

		Customer cc = new Customer { Unid = 1, CustomerName = "John" };
		string strJson = ScriptSerialize(cc);
		Console.WriteLine(strJson);
	
（二）反序列化

		public Customer ScriptDeserialize(string strJson)
		{
			JavaScriptSerializer js = new JavaScriptSerializer();
			return js.Deserialize<Customer>(strJson);
		}
	
通过Deserialize<T>方法来实现。
测试：

		Customer c1 = ScriptDeserialize(strJson);
		Console.WriteLine(c1.Unid + " " + c1.CustomerName);
	
（三）方法泛型

		public string ScriptSerialize<T>(T t)
		{
			JavaScriptSerializer js = new JavaScriptSerializer();
			return js.Serialize(t);
		}
		public T ScriptDeserialize<T>(string strJson)
		{
			JavaScriptSerializer js = new JavaScriptSerializer();
			return js.Deserialize<T>(strJson);
		}
	
测试：

		Customer cc = new Customer { Unid = 1, CustomerName = "John" };
		string strJson = ScriptSerialize<Customer>(cc);
		Console.WriteLine(strJson);
		Customer c1 = ScriptDeserialize<Customer>(strJson);
		Console.WriteLine(c1.Unid + " " + c1.CustomerName);
