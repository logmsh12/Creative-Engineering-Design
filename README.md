# 프로젝트 이름:스마트 설비 유지보수 및 에러 트래킹 프로그램

## 프로젝트 소개

이 프로젝트는 어떤 문제를 해결하기 위해 시작했습니다.

## 구현할 기능

- [ ] 핵심 기능 만들기
- [ ] 실행 화면 추가하기
- [ ] README 정리하기

## 참고 자료

- 참고 프로젝트: [SuperCMMS GitHub Repository](https://github.com/SuperCMMS/Open-Source-CMMS)
- 참고 프로젝트: [CMMS & OEE 모니터링 시스템](https://github.com/opensourceoeesoftware/CMMS-OEE-Software)
- 참고 문헌: [GitHub Docs - 마크다운 안내](https://docs.github.com)



## 이미지
*https://www.google.com/search?sca_esv=848c9d4d3f0b36ed&sxsrf=APpeQnvwQG_CU525YEPGH7BPkSIyrjbyxQ:1788411558670&udm=2&fbs=ABfTbFUgt-aXEFkBhPo84x72c1XoLHFs6TcISYM3FayWAWWA7x3ZDrzG9CGSPZ4zI2S4lLgTjQBjLg-w7evwgS4TepviScjIPAMtEaxTh0S1yu9k4UrcQJCgXwHMeBfgxr7xNlUFLVNeCeCynX-ozTGZrBswIckS3dS-cjdnPUCE-LCYFdSKJVZB9XXIT1-vw79wb-3JRwTEWx2kfo7WJ51ag-LUKSGHRg&q=%EC%8A%A4%EB%A7%88%ED%8A%B8+%EC%84%A4%EB%B9%84+%EC%9C%A0%EC%A7%80%EB%B3%B4%EC%88%98+%EB%B0%8F+%EC%97%90%EB%9F%AC+%ED%8A%B8%EB%9E%98%ED%82%B9+%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8&sa=X&ved=2ahUKEwi8sKqd0NGWAxX8jq8BHcI4A10QtKgLegQIGhAB&biw=851&bih=897&dpr=1.5#sv=CAMSURoyKhBlLVFkS3ZiOXQ1YzNWSHNNMg5RZEt2Yjl0NWMzVkhzTToOR2w4Qm1LcXlDYnU1ek0gBCoXCgFzEhBlLVFkS3ZiOXQ1YzNWSHNNGAEwARgHIImrwZgCSggQARgBIAEoAQ**

## 코드 블록
```java
public class ErrorTracker {
    public static void logError(String errorCode, String deviceName) {
        // 설비 에러 발생 시 경고 출력 및 기록
        System.out.println("[시스템 경고] " + deviceName + " 장비 점검 필요");
        System.out.println("발생 에러 코드: " + errorCode);
        
        // 추후 에러 내역 DB 저장 및 해결 가이드 매칭 기능 추가 예정
    }
}

## 실행 방법

​```text
실행 방법은 프로젝트가 진행되면서 추가할 예정입니다.
​```
