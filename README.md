# tianchi_rankinglist
get information of alibaba tianchi competion's ranking list 

Alibaba recently held a competion names "TianChi",because the real caculation is not allowed to share,now i am going to show the code to get the ranking list information by R.


library(rvest)
library(dplyr)
getdata<-function(page,urlwithoutpage){
  url=paste(urlwithoutpage,page,sep="") #这里输入没有页码的url
  web<-html(url,encoding="UTF-8") #读取数据，规定编码
  list_tianchi<-web %>% html_nodes("li.list-item")
  ranking<-list_tianchi%>% html_nodes("div.ranking p")%>%html_text() 
  member<-list_tianchi%>% html_nodes("div.member-box p")%>%html_text()
  team<-gsub("([\n ])","",list_tianchi%>% html_nodes("div.team-box p")%>%html_text())
  team<-team[nchar(team)!=0]
  score<-list_tianchi%>% html_nodes("div.score")%>%html_text()
  rateaccuracy<-list_tianchi%>% html_nodes("div.rate-accuracy")%>%html_text()
  raterecall<-list_tianchi%>% html_nodes("div.rate-recall")%>%html_text()
  besttime<-list_tianchi%>% html_nodes("div.best-time")%>%html_text()
  data.frame(member,team,score,rateaccuracy,raterecall,besttime)
  
}

url<-"http://tianchi.aliyun.com/competition/rankingList.htm?spm=0.0.0.0.f186Bh&season=0&raceId=1&pageIndex="
rm(f_result)
f_result<-data.frame()
for (i in 1:25){
  f_result<-rbind(f_result,getdata(i,url))
}
write.csv(f_result,"fr.csv")#succeed,yeah!
