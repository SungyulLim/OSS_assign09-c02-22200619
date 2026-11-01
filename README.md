# 9주차 과제 (영화 관리 시스템) — Weekly Review

## Deployment

- GitHub URL:https://github.com/2026-2-OSS/assign09-c02-22200619
- Vercel URL: https://assign09-c02-22200619.vercel.app/
- MockAPI Endpoint URL : https://6ae2c63cbbb54129229a60c6.mockapi.io/Movie

## Service

### 주제
#### 영화 관리 프로그램

### Field

| 필드 | 종류 |
| --- | --- |
| `id` | MockAPI가 자동으로 생성하는 영화 ID |
| `title` | 영화 제목 |
| `director` | 감독 |
| `genre` | 장르 |
| `country` | 국가 |
| `releaseDate` | 개봉일 |
| `createdAt` | 데이터 생성 시각 |

### CRUD 및 부가 기능

- **Create — POST:** 폼에 입력한 영화를 서버에 등록한다.
- **Read — GET:** 페이지 시작 시 영화 목록을 조회하고 화면에 표시한다.
- **Update — PUT:** 선택한 영화의 기존 정보를 폼에 불러와 수정한다.
- **Delete — DELETE:** `confirm()`으로 확인한 후 해당 영화를 삭제한다.
- 등록·수정·삭제 성공 후 목록을 다시 조회한다. 배열은 화면 표시와 검색에 사용하고, 실제 데이터 저장은 MockAPI에서 처리한다.
- 영화 제목을 `filter()`로 검색하며, 검색어의 앞뒤 공백과 영어 대소문자를 구분하지 않는다.
- 등록과 수정 모두 필수 입력 검사와 함께 제목 2~100자, 감독 이름 최대 50자, 개봉 연도 1888~2026년 검사를 적용한다.

## Key Learning

1. **fetch()와 REST API:** 같은 리소스라도 GET·POST·PUT·DELETE에 따라 수행하는 작업이 달라진다. 수정과 삭제에서는 URL에 영화 ID를 포함해 대상을 지정해야 한다.
2. **JSON과 JavaScript 객체:** `JSON.stringify()`로 객체를 JSON 문자열로 바꾸어 전송하고, `response.json()`으로 서버 응답을 JavaScript 값으로 읽는다. 받은 배열을 DOM 요소로 만들어 화면에 표시할 수 있다.
3. **async/await와 오류 처리:** 서버 통신 결과를 `await`로 기다리고 `try/catch`로 오류를 처리한다. HTTP 오류는 `response.ok`로 확인하며, `finally`를 사용하면 성공 여부와 관계없이 버튼 상태를 복구할 수 있다.

## AI 활용 및 코드 비중

- **AI 코드 비중:** 약 20%
- **Search Usage:** GPT-6 Astra
- **사용목적:** Fetch api의 작동 방식에 대해서 공부하고 구체적으로 crud 상에서 어떻게 이용 되는지 이론을 공부함 / 해당 코드를 구성하는 과정에서 PATCH가 작동하지 않았는데 이 부분이 어떤 오류인지를 분석함.
- **참고 또는 반영한 기능:** 

**PATCH 오류**

- 처음에는 강의자료처럼 `fetch()`의 메서드를 PATCH로 지정했지만, 수정 저장을 누르면 `Failed to fetch` 오류가 발생했다. Console에서 `Method PATCH is not allowed by Access-Control-Allow-Methods in preflight response`라는 메시지를 확인하고 AI에 원인 분석을 요청했다.
- 해당 오류는 해당 서버가 PATCH 요청을 막아두어서 브라우저가 수정 요청을 차단(CORS 오류 발생)했던 것임을 알 수 있었다.

- 따라서 서버의 허용 목록은 서버가 결정하는 부분이라 클라이언트(웹페이지)의 요청 헤더를 바꾸는 것만으로는 해결할 수 없었기 때문에 서버가 허용하는 방식 중 수정 기능인 PUT으로 바꿔서 사용했다.
- 코드 수정: updateMovie() 안의 method: "PATCH"를 method: "PUT"으로 변경했다.
- 배운점 : 에러가 났을 때 단순한 Failed to fetch 메시지만 보고 코드가 틀렸다고 판단하기보다 console에 나와있는 오류와 실제 서버 응답 헤더를 함께 확인해야 정확한 원인(CORS 및 허용 메서드 문제)을 찾을 수 있다는 것을 깨달았다.

## Problem & Solution

- **PATCH 요청의 CORS 오류:** 수정 시 `Failed to fetch`가 발생했다. Console과 서버의 사전 요청 응답을 확인한 결과 PATCH가 허용 메서드에 없었다. 과제에서 허용하는 PUT으로 수정 요청을 변경했다.
- **검색 결과 유지:** 목록을 다시 조회할 때 전체 데이터를 바로 출력하면 검색 조건이 풀렸다. GET 이후 `searchMovies()`를 호출하도록 변경해 현재 검색어를 다시 적용했다.

## Reflection
요청이 실패하고 오류가 나타났을때 내가 작성한 코드에 오류가 발생하였는지 만을 보는 것이 아니라 DevTools의 Console과 Network에서 요청 주소, 메서드, 응답을 확인하는 과정이 중요하다고 느꼈다. 