# NFL Running Backs Interactive Visualization

### Code for this project can be found at [this repository](https://github.com/a-camarillo/NFL-running-backs-2019)

Interactive features include displaying player name with their respective yards and attempts on hover, as well as, a slider tool to change the week.


![Players](https://raw.githubusercontent.com/a-camarillo/NFL-running-backs-2019/master/images/players.gif)
&nbsp;
&nbsp;


Clicking on a player's point will display their weekly rushing performances over the entire season. Weeks without a point represent a bye for that player whereas a point with 0 indicates the player did not gain any yards due to injury, poor performance, being benched, et cetera. 

![Clicks](https://raw.githubusercontent.com/a-camarillo/a-camarillo.github.io/master/projects/nfl-viz/images/click.gif)

## Data Collection and Cleaning

To collect the data used for this project, I used the python libraries [Requests](https://requests.readthedocs.io/en/master/) and [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) to scrape player statistics from NFL.com.

Requests allows python to easily make http requests and BeautifulSoup allows you to parse through and extract data from HTML an XML files.

Before scraping any website, you want to make sure you're not attempting to extract any information the website owners wouldn't want you to. To see what you're allowed and not allowed to scrape through you can simply use the url's `/robots.txt`.

<img src="https://github.com/a-camarillo/a-camarillo.github.io/blob/master/projects/nfl-viz/images/nfl_robots.png?raw=true" height="200" alt="robots">

Since the **Disallow:** does not include `/player` , the extension I used for player statistics, I am ok to scrape through without worrying about getting in trouble with NFL.com

