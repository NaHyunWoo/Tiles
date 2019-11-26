# Tiles

1. 라이브러리 다운로드 - pom.xml
1) properties 추가 
-------------------------------------------------------------------------------
<org.apache.tiles.version>2.2.2</org.apache.tiles.version>


2) dependency 추가
-------------------------------------------------------------------------------
<!-- tiles start -->
		<dependency> 
			<groupId>org.apache.tiles</groupId> 
			<artifactId>tiles-core</artifactId> 
			<version>${org.apache.tiles.version}</version> 
		</dependency> 
		<dependency> 
			<groupId>org.apache.tiles</groupId> 
			<artifactId>tiles-jsp</artifactId> 
			<version>${org.apache.tiles.version}</version>
		</dependency> 
		<dependency> 
			<groupId>org.apache.tiles</groupId> 
			<artifactId>tiles-api</artifactId> 
			<version>${org.apache.tiles.version}</version> 
		</dependency> 
		<dependency> 
			<groupId>org.apache.tiles</groupId> 
			<artifactId>tiles-servlet</artifactId>
			<version>${org.apache.tiles.version}</version> 
		</dependency> 
		<dependency> 
			<groupId>org.apache.tiles</groupId> 
			<artifactId>tiles-template</artifactId> 
			<version>${org.apache.tiles.version}</version> 
		</dependency>
		<!-- tiles end -->
===============================================

2.  환경설정 추가  :  servlet-context.xml
    ---> tiles 환경 설정 및 view 객체 생성
    ---> InternalResourceViewResolver  설정 부분은 주석 처리!!

<!-- URL로 넘어오는 주소를 매핑해서 사용하겠다 , 이때 사용하는게 TilesView -->
<!-- Tiles view  -->
	<beans:bean id="viewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver"> 
	<beans:property name="viewClass" value="org.springframework.web.servlet.view.tiles2.TilesView" /> 
	</beans:bean> 
	
	<!-- Tiles configuration --> 
	<beans:bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles2.TilesConfigurer"> 
		<beans:property name="definitions"> 
			<beans:list > 
			<beans:value>/WEB-INF/tiles/tiles.xml</beans:value> 
		</beans:list> 
		</beans:property> 
	</beans:bean>

===============================================

3.  /WEB-INF/tiles/tiles.xml  파일 생성
------------------------------------------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE tiles-definitions PUBLIC
  "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
  "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
 
<tiles-definitions>
 
    <!-- 화면의 기초가 되는 base 
    	template = "/WEB-INF/views/layout.jsp"에 형식을 만들어 두고 각 페이지를 조합함
    	name = "base"는 변경해도됨
    -->
    <definition name="base" template="/WEB-INF/views/layout.jsp">      
        <put-attribute name="header" value="/WEB-INF/views/header.jsp" />
        <put-attribute name="footer" value="/WEB-INF/views/footer.jsp" />
    </definition>
    
    <!-- base를 상속받은 base/index -->
    <!-- base를 상속받아 index.jsp를 body부분에 넣는다는 얘기 -->
    <definition name="base/index" extends="base">
        <put-attribute name="body" value="/WEB-INF/views/index.jsp" />
    </definition>
    
<!--
         동적 매핑(호출할때마다 바뀜).
         만약 /user/login의 경로라면 /WEB-INF/views/login/login.jsp 뷰를 가져온다. 
         케바케 * 를 넣으면 {1}을 넣고 jsp를 실행하겠다.(요청하는 url 주소가 아닌 Controller의 반환값을 기준으로 본다 
-->
    
    
    <!-- layout.jsp 안의  tiles:insertAttribute name="body"을 여기서 정의한
    	  즉 Controller에서 넘겨주는 jsp(view)를 body에 박는다  -->
    <!-- index -->     
    <definition name="*" extends="base">
        <put-attribute name="body" value="/WEB-INF/views/{1}.jsp" />
    </definition>
 	
 	<!-- /login/login -->
     <definition name="*/*" extends="base">
         <put-attribute name="body" value="/WEB-INF/views/{1}/{2}.jsp" />
     </definition>
    
    <definition name="*/*/*" extends="base">
        <put-attribute name="body" value="/WEB-INF/views/{1}/{2}/{3}.jsp" />
    </definition>
</tiles-definitions>

===============================================

4.  3번의 tiles.xml 파일에서 참조하는 ---->  /WEB-INF/views/layout.jsp 파일 생성
------------------------------------------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<!DOCTYPE html>
<html>
<head>
</head>
<body>
    <tiles:insertAttribute name="header" />
    <tiles:insertAttribute name="body" />
    <tiles:insertAttribute name="footer" />
</body>
</html>














