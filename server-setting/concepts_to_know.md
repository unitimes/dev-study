# Concepts to know
## Clustering vs. Load Balancing

### Clustering
	- 클러스터를 구성하는 자원들 간에는 서로의 상태를 앎 
	- 일반적으로 load balancing을 포함
	- 목적은 일반적으로 부하 조절 혹은 가용성을 높이는 것
### Load balancing
	- 복수 자원들간 부하를 조절 하는 것
### Apache - Tomcat
하나의 서버만을 구동할 것이면 apache와 같은 웹서버를 tomcat 앞에 구성하는 것은 과비용.
__Tomcat is__ - Servlet container의 역할, 일부 web server의 역할, jsp 파일을 servlet으로 파싱하는 역할을 함. 때문에 web server 없이 단독으로 서비스 가능. [참조](http://www.summa.com/blog/2011/05/16/why-a-web-server-the-benefits-of-apache)
__Apache is__ - 다수의 tomcat 구동, virtual hosting, load balancing, URL rewriting 등의 기능을 가짐. 이들 기능이 필요할 때 사용.