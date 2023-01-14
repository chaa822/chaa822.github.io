---
title: intellij 기본 설정
date: 2022-01-18
tags:
- settings
keywords:
- intellij 기본 설정
---

### 이 포스팅은 지속적으로 업데이트합니다.
<br/>
<br/>

#### 시작 화면 설정
* 프로그램 실행 시 프로젝트 선택 창 노출하도록
  <br/>Settings → Appearance > System Settings → Reopen projects on startup

#### 코드 스타일
* 들여쓰기 - 탭 사용
  <br/>Settings → Editor → Code Style → Java → Use tab character 체크, Smart tabs 체크 해제

#### Usage, New 마크 제거
* 에디터 내 Usage, New 마크 안보이게
  <br/>Settings → Editor → Inlay hints → Code vision 체크 해제

#### 자동 완성 (ctrl + space)
* 에디터 내에서 자동 완성이 안될 때
  <br/>Mac 환경설정 → 키보드 → 단축키탭 → 입력소스 → 이전 입력 소스 선택 체크 해제
  


#### Spring 
* Gradle 사용 시 
  <br/>Build and run using, Run tests using → IntelliJ IDEA
![](screenshot1.png)

#### MyBatis
* Mapper XML에 'No data sources are configured..' 경고가 뜰 경우
<br/>Settings > Editor > Inspections > SQL - No data sources configured, SQL dialect detection 체크 해제

#### Plugins
* Key Promoter X
* CodeGlance Pro
* Rainbow Brackets
* Quick File Preview

* MyBatisX
* JPA Buddy

* Dracula Pro
* Power mode 2
* Pokemon Progress
* Atom Material Icons
* Material Theme UI

