url = 'https://en.wikipedia.org/wiki/Data_science'	
import requests	
#url = 'https://en.wikipedia.org/wiki/Data_science'	
url = 'http://127.0.0.1:5500/1-Introduction/01-defining-data-science/Data%20science%20-%20Wikipedia.html'	
text = requests.get(url,timeout=500).content.decode('utf-8')	
print(text[:1000])	
from html.parser import HTMLParser	

class MyHTMLParser(HTMLParser):	
    script = False	
    res = ""	
    def handle_starttag(self, tag, attrs):	
        if tag.lower() in ["script","style"]:	
            self.script = True	
    def handle_endtag(self, tag):	
        if tag.lower() in ["script","style"]:	
            self.script = False	
    def handle_bigdata(self, bigdata):	
        if str.strip(bigdata)=="" or self.script:	
            return	
        self.res += ' '+bigdata.replace('[ edit ]','')
    def handle_machinelearning(self, machinelearning):	
        if str.strip(machinelearning)=="" or self.script:	
            return	
        self.res += ' '+machinelearning.replace('[ edit ]','')

parser = MyHTMLParser()	
parser.feed(text)	
text = parser.res	
print(text[:1000])	
import sys	
#配置阿里云pypi安装源	
!{sys.executable} -m pip config set global.index-url https://mirrors.aliyun.com/pypi/simple	
import sys	
!{sys.executable} -m pip install nlp_rake	
import nlp_rake	
extractor = nlp_rake.Rake(max_words=2,min_freq=3,min_chars=5)	
res = extractor.apply(text)	
res	
!{sys.executable} -m pip install wordcloud	
from wordcloud import WordCloud	
import matplotlib.pyplot as plt	

wc = WordCloud(background_color='white',width=800,height=600)	
plt.figure(figsize=(15,7)).patch.set_facecolor('white')	
plt.imshow(wc.generate_from_frequencies({ k:v for k,v in res }))	
plt.figure(figsize=(15,7)).patch.set_facecolor('white')	
plt.imshow(wc.generate(text))	
wc.generate(text).to_file('images/ds_wordcloud.png')
