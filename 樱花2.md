import time
import requests
from lxml import etree
import urllib.parse
def encode_text(text):
    # 使用 urllib.parse.quote 进行 URL 编码
    encoded_text = urllib.parse.quote(text, encoding='utf-8')
    return encoded_text
def get_url(region_name,headers):
    url = f"https://www.yhmgo.com/s_all?ex=1&kw={region_name}"
    print("总链接:")
    print(f"名称:{name},链接:{url}")
    print("==================================================================================================")
    url_request(url,headers)
def url_request(url, headers):
    #请求链接
    try:
        res = requests.get(url=url, headers=headers)
        res.raise_for_status()
        page = res.content.decode()
        html = etree.HTML(page)
        get_anime_url(html)
    except requests.exceptions.RequestException as e:
        print(f"Error fetching {url}: {e}")


def get_anime_url(html):
    #获取动漫链接
    url_list = html.xpath('//div[@class="fire l"]/div[@class="lpic"]/ul/li')
    for item in url_list:
        dm_titles = item.xpath('.//a/img/@alt')
        dm_urls = item.xpath('.//a/@href')
        if dm_titles and dm_urls:
            dm_title = dm_titles[0]
            dm_url = dm_urls[0]
            print(f"名称: {dm_title} 网址: https://www.yhmgo.com{dm_url}")
            print("==================================================================================================")
        else:
            print("No matching elements found")
if __name__ == '__main__':
    print("请输入想查找的动漫")
    name= input()
    region_name = encode_text(name)
    headers = {
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36",
    }
    get_url(region_name,headers)


