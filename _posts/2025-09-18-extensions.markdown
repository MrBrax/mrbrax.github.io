---
layout: post
title:  "Extensions"
date:   2025-09-18 17:29:04 +0200
categories: sbox
---

C# supports extensions, which allow you to add new methods to existing types without modifying their source code. This is done by creating a static class with static methods that take the type you want to extend as the first parameter.

Here are some of mine:

# NetList/NetDictonary
```csharp
public static class NetExtensions
{
	/// <summary>
	///  Convert a List to a NetList.
	/// </summary>
	/// <param name="list"></param>
	/// <typeparam name="T"></typeparam>
	/// <returns></returns>
	public static NetList<T> ToNetList<T>( this List<T> list )
	{
		var netList = new NetList<T>();
		foreach ( var item in list )
		{
			netList.Add( item );
		}

		return netList;
	}

	/// <summary>
	///  Convert an IEnumerable to a NetList.
	/// </summary>
	/// <param name="list"></param>
	/// <typeparam name="T"></typeparam>
	/// <returns></returns>
	public static NetList<T> ToNetList<T>( this IEnumerable<T> list )
	{
		var netList = new NetList<T>();
		foreach ( var item in list )
		{
			netList.Add( item );
		}

		return netList;
	}

	/// <summary>
	///  Convert a NetList to a List.
	/// </summary>
	/// <param name="dictionary"></param>
	/// <typeparam name="TKey"></typeparam>
	/// <typeparam name="TValue"></typeparam>
	/// <returns></returns>
	public static NetDictionary<TKey, TValue> ToNetDictionary<TKey, TValue>( this Dictionary<TKey, TValue> dictionary )
	{
		var netDictionary = new NetDictionary<TKey, TValue>();
		foreach ( var pair in dictionary )
		{
			netDictionary.Add( pair.Key, pair.Value );
		}

		return netDictionary;
	}

	/// <summary>
	///  Convert a NetDictionary to a Dictionary.
	/// </summary>
	/// <param name="netDictionary"></param>
	/// <typeparam name="TKey"></typeparam>
	/// <typeparam name="TValue"></typeparam>
	/// <returns></returns>
	public static Dictionary<TKey, TValue> ToRegularDictionary<TKey, TValue>(
		this NetDictionary<TKey, TValue> netDictionary )
	{
		var dictionary = new Dictionary<TKey, TValue>();
		foreach ( var pair in netDictionary )
		{
			dictionary.Add( pair.Key, pair.Value );
		}

		return dictionary;
	}

	/// <summary>
	///  Add a range of items to a NetList.
	/// </summary>
	/// <param name="netList"></param>
	/// <param name="list"></param>
	/// <typeparam name="T"></typeparam>
	public static void AddRange<T>( this NetList<T> netList, IEnumerable<T> list )
	{
		foreach ( var item in list )
		{
			netList.Add( item );
		}
	}

	/// <summary>
	///  Remove all items from a NetList using a predicate.
	/// </summary>
	public static void RemoveAll<T>( this NetList<T> netList, Func<T, bool> predicate )
	{
		var itemsToRemove = netList.Where( predicate ).ToList();
		foreach ( var item in itemsToRemove )
		{
			netList.Remove( item );
		}
	}
}
```