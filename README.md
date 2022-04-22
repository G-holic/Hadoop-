# Hadoop-
import requests  # 利于解析html
from bs4 import BeautifulSoup # 利于选取html的属性

#要爬取的网页
start_url = 'https://movie.douban.com/subject/27092785/comments?status=P' 
#  用户代理
headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36'}
# 获取网页内容
res = requests.get(start_url, headers=headers) # 传入网页和用户代理
# 获取网页的html
soup = BeautifulSoup(res.text,'html.parser')

# 分析文档
comments = soup.find_all('div',{'class':'comment-item'}) # 指定选取div标签下的类comment-item #soup.find_all找寻所有符合条件的数据

# 解析短评

# 定义几个列表，方便写入DataFrame
name_list = []
rating_list = []
time_list = []
content_list = []
vote_list = []

for comment in comments: # for循环迭代将内容传入comment
#     print(comment)

    #         name = comment.find('a')['title'] and comment.find('a')['title'] or '' # 防止数据有空值报错

    # 用户名信息
    name = comment.find('a')['title'] # 用find方法找寻相关元素 
    name_list.append(username)
    
    # 评分星级信息
#     rating = comment.find('span',{'class':'rating'})['title']
    # 另一种写法
#     rating = comment.find('span',class_='rating')['title']# class类不能直接用，需要加下划线
#     rating_list.append(rating)
    ## 因为有的人没有评论，所以有的没有这个标签，我们要对此作出判断，没有这个标签就为空，否则报错
    try:
            rating = comment.find('span',class_='rating')['title']
            if rating:
                ratings = rating
            else:
                ratings = ''
    except Exception:
        ratings = ''
    rating_list.append(ratings)  
        
    # 时间信息
    # 应对不是很规则的数据时
#     time = comment.find('span',class_='comment-time').text.split() # text,只取文本信息 。split()表示只删除空格信息
    time = comment.find('span',class_='comment-time')['title'].split()[0] # 只取第一个数据
    time_list.append(time)
    
    # s.strip(x):删除s字符串中开头、结尾处、位于x删除序列的字符
    # split是分割函数，将字符串分割成"字符"，保存在一个列表中，默认不带参数为空格分割
    
    # 评论信息
    content = comment.find('span',class_='short').text
    content_list.append(content)
    
    # 点赞数信息
    vote = comment.find('span', class_='vote-count').text
    vote_list.append(vote)
    
    # 把以上所有数据都合并
#     data = username + ';' + rating + ';' + time + ';' + content + ';' + vote + '\n'
#     print(data)
