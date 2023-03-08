Dependencies:
The crawler needs to run with Python 3.6 and later.
You need to install all the dependencies, such as bs4/beautifulsoup4, which is used for HTTP parsing.
requests_mock is used to mock the HTTP requests in unittest. 
Coverage is used for unit testing and to get the test coverage.

To install all the dependencies, you can run:
pip install -r requirements.txt

To crawl any website, you can run (for example):
python main.py "https://monzo.com/"

To run the tests:
python -m unittest 

The output will print all the URLs and the list of all connected URLs on the page. For example:
Current page: https://monzo.com/faq/
The connected URLs on the page: ['https://monzo.com/i/helping-everyone-belong-at-monzo/',...]
Current page: https://monzo.com/information-about-current-account-services/
The connected URLs on the page: ['https://monzo.com/i/helping-everyone-belong-at-monzo/',....]

The design of this app:
There is a file called crawler-flow.jpg, which describe how my web crawler works. 
It will help you to understand my thoughts.

This web crawler is a mini app. When you provide a small entry URL, the crawler will print all the URLs on the same domain, 
as well as all the links found on the pages.

This crawler will have two main storage components.
-One is called "urls_queue," where we will put the entry URL and all the validated URLs. Every time we take a URL from the queue, 
we fetch the HTML content, parse it, extract the URLs from the content, validate them, and put the validated URLs into the urls_queue.

-The other storage component is "visited_urls." Every time we extract a URL from urls_queue, we will add this URL to the visited_urls set. 
We do this because there might be a cycle, for example, we visit "a," get "b," then we visit "b" and get "a" again.

Some key points:

Concurrency:
The WebCrawler uses multiprocessing to achieve concurrency. Without multithreading, one thread needs to wait a long time 
(around one second on my local machine) to get a response. Multithreading can save waiting time for responses, 
allowing the CPU to execute other threads. Multithreading makes the app much more efficient.

Testing:
The unit tests cover 76% of the code, except lines 75-77, 81-84 in main.py (which is the args part; I don't think we need to test it :|).

Retry logic:
For the requirements, I chose a strategy of retrying three times to visit a website. I chose this strategy because I don't think it's really necessary to save the URL and revisit it in the future in this case.

Suggestions for future improvements:
Due to the requirements of this project and time constraints (4 hours :*), there are some areas that could be improved in the future. For example, allowing the user to decide the depth, maximum number of pages per domain, and execution time.