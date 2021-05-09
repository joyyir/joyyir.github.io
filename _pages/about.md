---
layout: wide
permalink: /about/
---
<!-- modern-resume-theme에서 생성된 _site/index.html의 body 붙여넣기 -->
<!-- /assets/main.css은 /assets/about/assets/main.css로 대체 -->

<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <title>Jang Junyeong | 11Street Back-end Software Engineer</title><!-- Begin Jekyll SEO tag v2.6.1 -->
<meta name="generator" content="Jekyll v3.9.0" />
<meta property="og:title" content="11Street Back-end Software Engineer" />
<meta property="og:locale" content="en_US" />
<link rel="canonical" href="http://localhost:4000/" />
<meta property="og:url" content="http://localhost:4000/" />
<meta property="og:site_name" content="11Street Back-end Software Engineer" />
<script type="application/ld+json">
{"headline":"11Street Back-end Software Engineer","url":"http://localhost:4000/","name":"11Street Back-end Software Engineer","@type":"WebSite","@context":"https://schema.org"}</script>
<!-- End Jekyll SEO tag -->
<link rel="stylesheet" href="/assets/about/assets/main.css">
  <link rel="shortcut icon" type="image/x-icon" href="/assets/about/assets/favicon.ico">
</head>
<body class=""><div class="container header-contianer">
  <div class="row">
    <div class="col-xs-12 col-sm-6 col-md-6 col-lg-8 header-left">
      <h1>Jang Junyeong</h1>
      <h2>11Street Back-end Software Engineer</h2>
    </div>
    <div class="col-xs-12 col-sm-6 col-md-6 col-lg-4 header-right">
      <ul class="icons no-print"><li>
            <a target="_blank" href="https://github.com/joyyir" class="button button--sacnite button--round-l">
              <i class="fab fa-github" title="Github link"></i>
            </a>
          </li><li>
            <a target="_blank" href="https://www.linkedin.com/in/joyyir" class="button button--sacnite button--round-l">
              <i class="fab fa-linkedin" title="Linkedin link"></i>
            </a>
          </li>
      </ul><p>
          Email: <a href="mailto:joyyir@naver.com" target="_blank">joyyir@naver.com</a>
        </p><p>
          Web: 
<a href="https://joyyir.github.io" target="_blank" class="">https://joyyir.github.io</a>
        </p></div>
  </div>
</div>
<main class="page-content" aria-label="Content">
    <div class="wrapper"><div class="container intro-container">
  <h3>About Me</h3>
  <div class="row clearfix"><div class="col-xs-12 col-sm-4 col-md-3 no-print">
        <span class="profile-img" style="background-image: url(/assets/about/assets/image/profile.jpg)"></span>
      </div><div class="col-xs-12 col-sm-8 col-md-9 col-print-12">
      <ul>
  <li>2017년 SK플래닛에 신입 공채로 입사하여, 현재는 11번가에서 전시 서비스 및 플랫폼 개발 업무를 담당하고 있는 5년차 백엔드 개발자 입니다.</li>
  <li>Java, Spring Framework 환경의 개발 경험이 많고, 최근에는 Batch, Kafka 관련 개발도 하고 있습니다.</li>
  <li>깔끔한 코드, 성능이 좋은 코드를 짜고 싶은 욕심이 있습니다!</li>
  <li>테스트 코드가 당연시되고 코드리뷰가 활발한 수평적인 분위기의 팀에서 일하고 싶습니다.</li>
</ul>

    </div>
  </div>
</div>

          <div class="container list-container">
            <h3>Projects</h3>
            
  <div class="row clearfix layout layout-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>11번가 전시 상품 데이터 플랫폼 개발</h4>
<a href="http://" target="_blank" class="link">2020.5 - 현재</a><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><h4 id="주요-내용">주요 내용</h4>
<ul>
  <li>11번가 전시 상품 데이터는 Oracle DB의 Stored Procedure가 일정 주기로 실행되며 반정규화 테이블에 추가/수정/삭제되는 구조였음. 이 구조에서는 Oracle DB와의 의존성을 끊을 수 없는 상태이고, 일정 주기로 갱신이 이루어지기 때문에 실시간성이 떨어짐</li>
  <li>이를 개선하기 위해 이벤트 기반으로 상품 데이터의 실시간 갱신이 가능하고 Oracle DB에 종속되지 않고 어느 DBMS로도 변경할 수 있는 시스템을 구축하였음</li>
</ul>

<h4 id="본인이-기여한-점">본인이 기여한 점</h4>
<ul>
  <li>팀원들과 함께 논의하여 전체 프로세스 설계</li>
  <li>Spring Kafka 기반 Consumer Application 개발</li>
  <li>Kafka JDBC Connector를 구축하여 RDBMS 데이터 변경 분을 Kafka 메세지로 생성</li>
  <li>Kafka Broker/Consumer/Producer의 다양한 옵션에 대한 research 및 performance test</li>
  <li>ELK, Grafana, Metricbeat를 활용한 모니터링 환경 구축</li>
  <li>기존 데이터와 신규 데이터의 비교 및 검증을 위한 Validator 구현 및 Grafana를 통한 시각화</li>
  <li>Spring Batch 기반 전시 상품 데일리 갱신을 위한 Batch Application 개발</li>
</ul>

<h4 id="사용한-기술스택-및-지식">사용한 기술스택 및 지식</h4>
<ul>
  <li>Java 11, Spring Kafka, Spring Data MongoDB, Spring Batch</li>
  <li>Kafka Broker/Producer/Consumer/JDBC Connector</li>
  <li>MongoDB</li>
  <li>Elasticsearch, Logstash, Kibana, Metricbeat, Grafana</li>
</ul>

<h4 id="결과-및-성과">결과 및 성과</h4>
<ul>
  <li>2021년 상반기 내 상용 서비스 예정</li>
  <li>이벤트 기반 갱신으로 기존 대비 빠른 데이터 갱신</li>
  <li>고비용의 Oracle DB에서 MongoDB를 사용함으로 인한 비용 절감</li>
  <li>이벤트 기반 아키텍처의 이점인 느슨한 결합, 내구성, 확장성, 유연성 제공</li>
</ul>

    </div>
  </div>

  <div class="row clearfix layout layout-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>11번가 전시 서비스 개발</h4>
<a href="http://" target="_blank" class="link">2017.3 - 2020.4</a><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><h4 id="주요-내용">주요 내용</h4>
<ul>
  <li>11번가 모바일 메인탭의 External Web API 개발 (프론트엔드에 제공)</li>
  <li>사내 서버 간 데이터 연동을 위한 Internal Web API 개발</li>
  <li>레거시 코드의 리팩토링, 성능 개선, 신규 프로젝트로 이관</li>
  <li>전시 플랫폼의 코어 로직 신규 개발 (영역 간 데이터 공유 기능, 페이징 기능 등)</li>
  <li>애플리케이션 오류 모니터링, 운영 이슈 대응</li>
</ul>

<h4 id="본인이-기여한-점">본인이 기여한 점</h4>
<ul>
  <li>다양한 주체와 원만하고 명료한 커뮤니케이션 (기획자, 디자이너, 다른 팀 서버 개발자, 프론트엔드 개발자, QA 담당자)</li>
  <li>단위 테스트를 작성하여 변경에도 안전하도록 신뢰성 보장</li>
  <li>응답 시간이 높은 API에 대한 병목 원인을 분석하고 다양한 방법으로 성능 개선 (캐싱, Thread Pool을 통한 병렬 처리 등)</li>
  <li>DDD에서 설명하는 Layer의 역할별로 코드를 리팩토링, 의존성 주입이 가능한 형태로 코드를 리팩토링</li>
</ul>

<h4 id="사용한-기술스택-및-지식">사용한 기술스택 및 지식</h4>
<ul>
  <li>Java 8, Spring Boot, JPA(Spring Data JPA, Querydsl), MyBatis, JUnit 5, RxJava, Caffeine (in-memory cache), Oracle DB SQL</li>
  <li>async-profiler를 활용한 성능 프로파일링, Apache JMeter를 활용한 서버 부하 생성</li>
</ul>

<h4 id="결과-및-성과">결과 및 성과</h4>
<ul>
  <li>완성도 있는 서비스를 일정을 준수하여 개발 및 배포하여 회사 매출에 기여</li>
  <li>API 응답 시간 개선으로 사용자 경험 향상, 서버 리소스를 효율적으로 운영</li>
  <li>리팩토링을 통해 코드의 가독성, 재사용성, 테스트가능성을 높임</li>
  <li>전시 플랫폼의 코어 로직 개선으로 다양한 기획 요구사항을 수용 가능하게 되었음
    <ul>
      <li>영역 간 데이터 공유가 가능하게 되어 복잡한 UI로 디자인된 영역을 개발할 수 있게 되었음</li>
      <li>페이징 기능을 베스트탭, 꾹꾹탭에 적용하여 서버 응답시간을 단축하면서도 화면에 더 많은 상품을 보여줄 수 있게 되었음</li>
    </ul>
  </li>
</ul>

    </div>
  </div>


          </div>
        
          <div class="container list-container">
            <h3>Experience</h3>
            
  <div class="row clearfix layout layout-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>11번가</h4><p><b>Manager</b></p><p>2018.9 - 현재</p>
<a href="https://www.11stcorp.com/" target="_blank" class="link">https://www.11stcorp.com/</a><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><p class="quote">2018.9 SK플래닛에서 분사
</p><p>11번가 전시 서비스 및 플랫폼 개발</p>

    </div>
  </div>

  <div class="row clearfix layout layout-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>SK Planet</h4><p><b>Manager</b></p><p>2017.1 - 2018.8</p>
<a href="https://www.skplanet.com/" target="_blank" class="link">https://www.skplanet.com/</a><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><p>11번가 전시 서비스 개발</p>

    </div>
  </div>


          </div>
        
          <div class="container list-container">
            <h3>Education</h3>
            
  <div class="row clearfix layout layout-top-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>KOREATECH</h4><p><b>한국기술교육대학교 컴퓨터공학부</b></p><p>2011.3 - 2017.2</p><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><ul>
  <li>학사 졸업</li>
  <li>평점 평균 4.39/4.5</li>
</ul>

    </div>
  </div>

  <div class="row clearfix layout layout-top-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>Coursera</h4><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><ul>
  <li><a href="https://www.coursera.org/account/accomplishments/verify/7BBVZQEMJ4HJ">Machine Learning (Stanford University)</a></li>
  <li><a href="https://www.coursera.org/account/accomplishments/verify/YV7R2KGPPYCV">HTML, CSS, and Javascript for Web Developers (Johns Hopkins University)</a></li>
</ul>

    </div>
  </div>

  <div class="row clearfix layout layout-top-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>Udemy</h4><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><ul>
  <li><a href="https://www.udemy.com/certificate/UC-f5560c17-6a94-425f-bf1a-562cc1405d4e/?utm_campaign=email&amp;utm_source=sendgrid.com&amp;utm_medium=email">Apache Kafka Series - Learn Apache Kafka for Beginners v2</a></li>
  <li><a href="https://www.udemy.com/certificate/UC-a81a41fc-a5e5-4ae1-b2d1-abd960f71bfb/">MongoDB - The Complete Developer’s Guide 2020</a></li>
  <li><a href="https://www.udemy.com/certificate/UC-59a98f77-aed6-4e04-ac7a-8ddb919e7b11/">Domain Driven Design: Complete Software Architecture Course</a></li>
</ul>

    </div>
  </div>

  <div class="row clearfix layout layout-top-left">
    <div class="col-xs-12 col-sm-4 col-md-3 col-print-12 details">
      <h4>Inflearn</h4><p class="no-print aditional-links">
        
      </p>
    </div>
    <div class="col-xs-12 col-sm-8 col-md-9 col-print-12 content"><ul>
  <li><a href="https://inf.run/z8mC">스프링 데이터 JPA</a></li>
  <li><a href="https://joyyir.github.io/front-end/vue-js-inflearn/">실습 UI 개발로 배워보는 순수 javascript 와 VueJS 개발</a></li>
</ul>

    </div>
  </div>


          </div>
        
          <div class="container text-container">
            <h3>Publication</h3>
            <div class="row clearfix">
  <div class="col-md-12">
    <ul>
  <li><a href="http://www.riss.kr/link?id=A103110843">S-FDS : 퍼지로직과 딥러닝 통합 기반의 스마트 화재감지 시스템, 대한전자공학회 논문지, 2017</a></li>
</ul>

  </div>
</div>

          </div>
        
          <div class="container text-container">
            <h3>A Little More About Me</h3>
            <div class="row clearfix">
  <div class="col-md-12">
    <ul>
  <li><a href="https://joyyir.github.io/">개인 개발 블로그</a></li>
  <li><a href="https://drive.google.com/file/d/0B-Q3HcQtLz_8RjZmdk5WOEFPSkE/view?usp=sharing">2016 포트폴리오 pdf</a></li>
</ul>

  </div>
</div>

          </div>
        
      </div>
  </main><div class="container footer-container">
  <p>
    Jang Junyeong -
    <a href="mailto:joyyir@naver.com" target="_blank">joyyir@naver.com</a>&nbsp;- References on request</p>
</div>
</body>
