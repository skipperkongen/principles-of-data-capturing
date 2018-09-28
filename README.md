# Principles of web analytics for data science

Many companies want to improve their business by capturing the data from their systems, e.g. they might track the behaviour of customers on the company website to discover their needs. Capturing data and events helps the company make decisions that are based on facts instead of speculation.

However, the opinion on exactly what data is essential to the business will depend on who you ask. Your business intelligence team might say that key performance indicators (KPI) and certain types of aggregated data, e.g. purchases per day, are essential to their reporting. Your data science team might tell you that very granular data on user behaviour is essential for their predictive modelling.

Now, imagine that you are about to implement data tracking on your company's website that will make everyone happy. What are some key principles that you should follow?


## Describe what happened

> Who, what, when, where

> sufficient to identify the customer and bucket their needs....


> sequentially, what do you mean. Placing clearly in our boioking context, prefarbly witoth know the whole process or implemeation.

> The before and after might be important, i.e. causality might be important

People one hundred years in the future or in the past should be able to make sense of your events. These people don't care whether your website was implemented in Wordpress or Django CMS. They care about what logically happened, e.g. that a purchase was made.

If it helps, imagine that your events are letters that you write to analysts in a distant future, who doesn't know about The Internet.

For example, the information that a customer bought a broomstick from your shop at two o'clock in the morning in Copenhagen, should theoretically be understood the same in the years 1820, 2018 and 2108. Conversely, the fact that a customer browsed the url `x/y/z/` may hold no meaning to people in the future. Crucially, the information captured by the event should not depend on your use of technology today. For example, the URLs that collectively make up your website hold no special meaning for people in the future. Furthermore, urls are things that are subject to change and therefore should not be the prime carrier of information about what happened. Simply put, your product search page may move from `/products/search.py` to `modules/search.php` but future analysts shouldn't have to worry about that.

Did a customer search for "broomstick" on the product page of the website goldenproducts.com at two o'clock in the morning in Copenhagen? Then your event should as a minimum include something like this:

```
{
    "userID": 1907834183291,
    "sessionID": 493284092384,
    "timestamp": "2018-09-27T02:01:01Z",
    "action": "product-search",
    "query": "broomstick",
    "shop": "goldenproducts",
    "cityName: "Copenhagen",
    "countryCode": "DK",
    "longitude": 12.56,
    "latitude": 55.48,
}
```

Notice that this information does not depend on implementation details, which may change. For example, the url of the product may change if someone fixes a spelling mistakes in the path (e.g. `products/seach` -> `products/search`) or new technology tried out (e.g. Django instead of Wordpress to power the website). Ok, maybe your UX people want to know implementation-specific details, such as the url where an action took place. It doesn't hurt to add it, but it should not replace the logical description of what happened. e.g. `https://goldenproducts.com/product/seach?term=broomstick`, but only after you captured everything that does not depend on the implementation details.

Therefore, every event you capture should be an unambiguous and self-explanatory piece of evidence that accurately records relevant details of what happened at a particular point in time and space, without reference to implementation-specific details. That means the who, what, where and when of what happened.

## Aggregate later

It's a basic fact of data processing that you can turn granular data into aggregate data, but not the other way around. While you can count the number of purchases per day from a list of purchases, you can't derive a list of purchases from a count.

Therefore, you should capture granular data.

## Use unique identifiers

Always use unique identifiers (large, dumb numbers) to refer to the who, what, where and when of your events - whenever possible. For example, if a customer bought something, then the event should record the ID of the user and the ID of the product.

Identifiers in your events unambiguously point to records in your business database. If you want to know the name and manufacturer of the product the customer bought, then look up the product by ID in the database and check those fields. The event should not carry this information, since the list of potentially useful information is infinite.

## Identifiers should not contain hidden meaning

Never embed meaning in your identifiers. An identifier is a dumb number with the only property that it uniquely identifies something in your business database or the external world. If you need to say that a customer belongs to a country, then add a country field to the database, don't be an idiot and add a special country prefix to your customer IDs. For example, you would never add an "r" prefix to customer IDs for red-headed customers, would you? If hair color is important to you, then add a hair color field to the customer database.

## Use standard codes for countries, cities and currencies

## Specify the unit of measurement

> Use "EUR not "euros, use "USD" not "$"

> E.g. WGS84 for coordinates

> Be consistent

There are cases where it seems farfetched to use an identifier, e.g. in the domain of floating point numbers that represent geographical coordinates. There, simply use the floating point coordinates since you can't mathematically enumerate all distinct coordinates. Same for timestamps. There are cases, e.g. city names, which could represented by an ID, but where you could arguably get away with using city names instead of city IDs, but these cases should all but be elimitated.

## Use grow-only schemas

## Use easy-to-parse formats (e.g. JSON)

Even if your events should be understandable in 100 years, do use formats that people in the present can easily parse. Therefore, when you write events, you should use a format that is easy to parse by most programmers today. Your consumers (who are analysts or programmers) will rely on standard parsers, so use a standard data format like JSON, Protocol Buffers or XML. Never, ever use home-cooked formats that requires people to guess what means what.

## Conclusion

Follow the above principles and your events will be relevant for analysis for all eternity.

## Acknowledgements

Iain Morgan for challenging my terminology and adding more principles, such as capture units.

