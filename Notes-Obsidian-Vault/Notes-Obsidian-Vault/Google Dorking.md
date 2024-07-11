https://pagespeed.web.dev/


### Robots.txt

Similar to "Sitemaps" which we will later discuss, this file is the first thing indexed by "Crawlers" when visiting a website.

  

#### But what is it?

This file must be served at the root directory - specified by the webserver itself. Looking at this files extension of **.txt**, its fairly safe to assume that it is a text file.

The text file defines the permissions the "Crawler" has to the website. For example, what type of "Crawler" is allowed (I.e. You only want Google's "Crawler" to index your site and not MSN's). Moreover, Robots.txt can specify what files and directories that we do or don't want to be indexed by the "Crawler".

A very basic markup of a Robots.txt is like the following:

![](https://i.imgur.com/wZ3lo4B.png)  

  

Here we have a few keywords...

|   |   |
|---|---|
|Keyword|Function|
|User-agent|Specify the type of "Crawler" that can index your site (the asterisk being a wildcard, allowing **all "User-agents"**|
|Allow|Specify the directories or file(s) that the "Crawler" **can** index|
|Disallow|Specify the directories or file(s) that the "Crawler" **cannot** index|
|Sitemap|Provide a reference to where the sitemap is located (improves SEO as previously discussed, we'll come to sitemaps in the next task)|

In this case:
1. Any "Crawler" can index the site
2. The "Crawler" is allowed to index the entire contents of the site
3. The "Sitemap" is located at [http://mywebsite.com/sitemap.xml](http://mywebsite.com/sitemap.xml)[](http://mywebsite.com/sitemap.xml)
  
Say we wanted to hide directories or files from a "Crawler"? Robots.txt works on a "blacklisting" basis. Essentially, **unless told otherwise**, the Crawler will index whatever it can find.

![](https://i.imgur.com/audlFn8.png)

In this case:
1. Any "Crawler" can index the site  
2. The "Crawler" can index every other content that isn't contained within "/super-secret-directory/".

Crawlers also know the differences between sub-directories, directories and files. Such as in the case of the second "Disallow:" ("/not-a-secret/but-this-is/")

The "Crawler" will index all the contents within "**/not-a-secret/**", but will not index anything contained within the sub-directory **"/but-this-is/"**.
3. The "Sitemap" is located at [http://mywebsite.com/sitemap.xml](http://mywebsite.com/sitemap.xml)

What if we Only Wanted Certain "Crawlers" to Index our Site?

We can stipulate so, such as in the picture below:

![](https://i.imgur.com/LxitBJs.png)

In this case:
1. The "Crawler" "Googlebot" is allowed to index the entire site ("Allow: /")
2. The "Crawler" "msnbot" is not allowed to index the site (Disallow: /")

  

How about Preventing Files From Being Indexed? 

Whilst you can make manual entries for every file extension that you don't want to be indexed, you will have to provide the directory it is within, as well as the full filename. Imagine if you had a huge site! What a pain...Here's where we can use a bit of [regexing](https://www.rexegg.com/regex-quickstart.html).

![](https://i.imgur.com/mzDqFVY.png)

In this case:
1. Any "Crawler" can index the site
2. However, the "Crawler" cannot index **any** file that has the extension of **.ini** within any directory/sub-directory using ("$") of the site.
3. The "Sitemap" is located at [http://mywebsite.com/sitemap.xml](http://mywebsite.com/sitemap.xml)
Why would you want to hide a **.ini** file for example? Well, files like this contain sensitive configuration details. Can you think of any other file formats that might contain sensitive information?


## Sitemaps

Comparable to geographical maps in real life, “Sitemaps” are just that - but for websites!

“Sitemaps” are indicative resources that are helpful for crawlers, as they specify the necessary routes to find content on the domain. The below illustration is a good example of the structure of a website, and how it may look on a "Sitemap":

![](https://i.imgur.com/L5WqJU4.png)

  

The blue rectangles represent the **route** to nested-content, similar to a directory I.e. “Products” for a store. Whereas, the green rounded-rectangles represent an actual page. However, this is for illustration purposes only - “Sitemaps” don't look like this in the real world. They look something much more similar to this:  

  

![](https://i.imgur.com/12Yxcn5.png)

“Sitemaps” are XML formatted. I won't explain the structure of this file-formatting as the room [XXE](https://tryhackme.com/jr/xxe) created by [falconfeast](https://tryhackme.com/p/falconfeast) does a mighty fine job of this.

The presence of "Sitemaps" holds a fair amount of weight in influencing the "optimisation" and favorability of a website. As we discussed in the "Search Engine Optimisation" task, these maps make the traversal of content much easier for the crawler!


