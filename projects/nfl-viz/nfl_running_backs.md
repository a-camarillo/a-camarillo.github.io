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

The function I wrote to scrape each player's gamelogs utilizes the libraries `requests` and `BeautifulSoup` which is aliased as `soup`.

```python
def acquire_stats(url):
    r = requests.get(url)
    c = r.content

    page = soup(c,'html.parser')

    #data-table1 is the standard class for NFL.com stats tables 
    reg = page.findAll("table",{"class":"data-table1"})
    for element in reg:
        if reg.index(element) == 1:
            table = element.find("tbody").findAll("tr") #for multiple tables in game logs, index 1 gives regular season
        elif len(reg) < 2:
            table = element.find("tbody").findAll("tr") #some pages only have regular season table
            

    #empty lists to append values to
    rows = []
    log = [] 

    #create a list with all of the td elements in the table
    for row in table:
        rows.append(row.findAll("td"))
    #iterate through the lists and retrieve the text elements, also removing line spacing characters
    for i in range(0,len(rows)):
        log.append([td.text.replace('\n','').replace('\t','').replace('\r','').replace('\xa0','').replace('@','') for td in rows[i]])
    del log[1::2] 
    del reg
    del table
    return log
```
The function first passes in a url and uses requests to get the content of the specified url. BeautifulSoup is used to create a "soup" object from the HTML content which can then be easily parsed. Next, the function looks for all `table` elements which have the class `"data-table1"` since this is the uniform class given to all of NFL.com's tables as seen below. 

<img src="https://raw.githubusercontent.com/a-camarillo/a-camarillo.github.io/master/projects/nfl-viz/images/nfl_tables.gif" height="500">

Since `findAll` creates a list of all of the elements it finds and I was only interested in looking at regular season statistics, the function assigns a value from the second element of the list. However, I found out that some player's gamelogs only have a regular season table so the `elif` statement accounts for those players as well. The function then grabs all of the table rows and assigns it to a variable. 

Finally the function iterates over the table rows, finding and cleaning all of the row elements, and appending all of the row elements to a list for the function to return.

I then created a list of urls from the top 50 rushers and used list comprehension, as well as the `dict` function, to create a dictionary where the keys are the player names and the values are nested lists containing the player's weekly statistics.

*Below is the first two weeks of Derrick Henry's stats to show what the dict looks like*
```python
{'Derrick Henry': [['1','09/08','  CLE','W    43-13 ','1','1','19','84','4.4','18','1','1','75','75.0','75T','1','--','--'],
['2','09/15','  IND','L   17-19 ','1','1','15','82','5.5','18','1','2','12','6.0','9','0','1','0'],
```

