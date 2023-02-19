# Overview

This is a document solution for the tasks:
 1. Suggest **improvements** for the website [Top 10 Boarding schools in Dehradun 2023-24 | Updated List](task_1)  that enchance **page load speed**.
 2. Suggest **technical improvements** for the website [Top 21 boarding schools in India 2023-24 | Updated Listing](task_2)
# ***Task 1***
# Issues

On inspecting the network activity of the desktop version website on pageload, It was found that the **initial pageload** time before any caching was possible was around $12$  seconds on average, and **after caching**/initial loading of the website pageload time was found to be around $2.5$ seconds 
on average, which is not bad as it varies with disk and memory read speed of the end user but can still be improved. 

## **Causes of initial page load delay:**

*Figure 1*: Initial pageload network analytics

![Initial pageload network analytics][initial]

### ***Too many server requests***
The webpage makes too many requests to the host server and this causes pageload to take time as the server is not that high-end to handle many request at a high rate, for eg:
* In the figure, the requests like `ec-mobile-1.jpg`, `jquery.min.css`, multiple font imports, image files all made to the host server take up most of the Load time.
* The server response is also delayed due to all these simultaneous requests on pageload

### ***Not using CDN to import scripts/stylesheets***
CDN link tags are much faster, have better caching, and reduce the load on server for serving existing popular libraries such as jquery, and for popular fonts such as Roboto ..etc.

### ***Loading images on site load***
Loading images take up most of the load time as shown in figure 1, 
Each image takes time in order of seconds, Other methods of loading images can be used to delay the loading of images to after the site has loaded

### ***External site plugins***
Google tag manager and other plugins can slow down the page load process and can be optimised in some ways.

## **With caching enabled:**

*Figure 2* : Network analytics with caching enabled

![caching enabled analytics][caching]

Here the figure shows data after reloading / revisiting the website on a desktop which already visited the website atleast one time. 
The site takes an average amount of time to load and can still be improved by reducing the no. of css / font imports and better loading method of images.

## *Mobile Version*
The mobile version has an average initial load time of $10.5$ seconds less than $12$ of desktop version, due to lesser number of font imports and simutaneous server requests but still can be greatly improved due to reasons mentioned above.

# ***Solutions***
## ***1. Importing using CDN***
fonts and external js libraries like jquery can be imported using official cdn tags provided by popular websited for eg:
```html
<script
  src="https://code.jquery.com/jquery-3.6.3.min.js"
  integrity="sha256-pvPw+upLPUjgMXY0G+8O0xUf+/Im1MZjXxxgOcBQBXU="
  crossorigin="anonymous">
</script>
```
Which can reduce server load and increase response speed

### ***Font imports***
font imports such as roboto and fontawesome can be imported using official cdn or from google fonts, for eg:

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.3.0/css/all.min.css" integrity="sha512-SzlrxWUlpfuzQ+pcUCosxcglQRNAq/DZjVsC0lE40xsADsfeQoEypE+enwcOiGjk/bSuGGKHEyjSoQ1zVisanQ==" crossorigin="anonymous" referrerpolicy="no-referrer" />
```

for multiple font weights instead of serving each font weight file from the server, cdn can be used along with css as shown:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,300;0,400;0,700;0,900;1,100&display=swap" rel="stylesheet">
```
This code imports multiple weights in single tag,now in the css part:

```css
font-family: 'Roboto', sans-serif;
font-weight: 700
```
the font weight can specified as per the available imports, this method is faster than doing multiple imports as shown:

```html
<link id='omgf-preload-0' rel='preload' href='//www.ecoleglobale.com/blog/wp-content/uploads/omgf/oceanwp-google-font-roboto/roboto-normal-100.woff2' as='font' type='font/woff2' crossorigin />
<link id='omgf-preload-1' rel='preload' href='//www.ecoleglobale.com/blog/wp-content/uploads/omgf/oceanwp-google-font-roboto/roboto-normal-300.woff2' as='font' type='font/woff2' crossorigin />
<link id='omgf-preload-2' rel='preload' href='//www.ecoleglobale.com/blog/wp-content/uploads/omgf/oceanwp-google-font-roboto/roboto-normal-400.woff2' as='font' type='font/woff2' crossorigin />
<link id='omgf-preload-3' rel='preload' href='//www.ecoleglobale.com/blog/wp-content/uploads/omgf/oceanwp-google-font-roboto/roboto-normal-500.woff2' as='font' type='font/woff2' crossorigin />
<link id='omgf-preload-4' rel='preload' href='//www.ecoleglobale.com/blog/wp-content/uploads/omgf/oceanwp-google-font-roboto/roboto-normal-700.woff2' as='font' type='font/woff2' crossorigin />
<link id='omgf-preload-5' rel='preload' href='//www.ecoleglobale.com/blog/wp-content/uploads/omgf/oceanwp-google-font-roboto/roboto-normal-900.woff2' as='font' type='font/woff2' crossorigin />
```
## ***2. Skeleton loading of images***
The website already implements lazy loading of non visible images, implementing delayed skeleton loading of images to after the site document along with stylesheets, scripts and fonts will further improve initial as well as subsequent site load time.

This will not cause any problem for the UI, and is a proven method used by many sites such as youtube, medium ..etc

## ***3. Using External CSS library***
importing a single minified css library such as **Bootstrap** or **Tailwind** via cdn can further reduce the server load by reducing the no. of stylesheets that need to be served by the backend. this will also reduce data traffic.
## ***4. Lazy loading Google analytics***
Implementing lazy loading for google analytics as suggested by [this article](article_1) will help improve page speed and performance, this can be done by adding the following script tag above the google tag manager script tag:

```html
<script>
    function analyticsOnScroll() {
        var head = document.getElementsByTagName('head')[0]
        var script = document.createElement('script')
        script.type = 'text/javascript';
        script.src = 'https://www.googletagmanager.com/gtag/js?id=G-XXXXXX'
        head.appendChild(script);
        document.removeEventListener('scroll', analyticsOnScroll);
    };
    document.addEventListener('scroll', analyticsOnScroll);
</script>
```

# Conclusion
The solutions $1$ and $2$ significantly decrease the no. of requests send to the host server and implementing them will improve initial site load speed by a considerable amount.

---

# ***Task 2***
# Possible Technical Improvements
## 1. Improve page load speed
The page has a slow webpage loading similar to the website shown in [Task 1](#task-1) due to the same reasons as mentioned in task 1 such as:
* [Too many server requests](#too-many-server-requests)
* [Not using CDN wherever needed](#not-using-cdn-to-import-scriptsstylesheets)
* [Loading images on site load](#loading-images-on-site-load)
* [External site plugins](#external-site-plugins)

These issue can be solved by the [Task 1 solutions](#solutions)
## 2. Search Engine Optimization (SEO)
SEO methods can be used to improve the site ranking on google search results and enchance the viewership of the website,
Some methods include:
* **Using Image alt tags:** adding various tags to improve search engine visibilty
* **Using Keywords:** using keywords that attract more users to the site, often throughout the site
* **get listed on Google's My Business**
## 3. UI can be more smooth and modern
* Using external CSS libraries such as ***Tailwind*** as suggested earlier the UI can be given a more modern and user-friendly look.
* Implementing more popular, UX-enhancing image loading techniques such as ***Skeleton loading*** which is implemented by top websites such as Youtube and Medium will improve the overall site significantly.
 




[task_1]: https://www.ecoleglobale.com/blog/best-boarding-schools-in-dehradun/
[task_2]: https://www.ecoleglobale.com/blog/top-21-boarding-schools-in-india/
[article_1]: https://michaelmannucci.com/blog/how-to-lazy-load-google-analytics

[initial]: https://raw.githubusercontent.com/AqueelAhmedV/markdown-files/main/assets/img_4.png

[caching]: https://raw.githubusercontent.com/AqueelAhmedV/markdown-files/main/assets/img_7.png