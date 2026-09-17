# 프로젝트 이름:맞춤형 조립 PC 견적 추천 및 호환성 체크 챗봇 프로젝트

## 프로젝트 소개

이 프로젝트는 단순히 "100만 원짜리 PC 맞춰줘"에 대답하는 것을 넘어, 사용자의 구체적인 목적과 예산에 맞춰 부품을 조합하고 병목 현상이나 호환성까지 조언해 주는 똑똑한 챗봇입니다.

## 구현할 기능

페르소나 및 역할 부여: 앞서 작성한 파이썬 코드의 System 프롬프트를 수정하여 LLM에 "당신은 조립 PC 전문가입니다. 사용자의 예산과 주 사용 목적을 파악하여 최적의 부품 조합을 제안합니다."라는 역할을 줍니다.

핵심 키워드 질의응답: "게임용 PC", "사무용 PC" 등 단순한 질문을 던졌을 때, 챗봇이 CPU, 그래픽카드(VGA), RAM, 파워 등의 대략적인 가이드라인을 텍스트로 잘 출력하는지 테스트합니다.



## 참고 자료

- 참고 프로젝트: [SuperCMMS GitHub Repository](https://github.com/SuperCMMS/Open-Source-CMMS)
- 참고 프로젝트: [CMMS & OEE 모니터링 시스템](https://github.com/opensourceoeesoftware/CMMS-OEE-Software)
- 참고 문헌: [GitHub Docs - 마크다운 안내](https://docs.github.com)



## 이미지
추가예정입니다.<img width="1344" height="1509" alt="image" src="https://github.com/user-attachments/assets/46838afa-1925-4a2b-9cab-fd8a8a307999" />


# 1. API 키 불러오기
from pathlib import Path
from openai import OpenAI

# 기존 코드의 설정입니다. 계정에서 이용 가능한 모델인지 확인하세요.
API_BASE_URL = "https://factchat-cloud.mindlogic.ai/v1/gateway"
MODEL_NAME = "claude-sonnet-5"

SYSTEM_PROMPT = """
당신은 20년 경력의 조립 PC 견적 전문가입니다.
1. 사용자의 예산과 주요 사용 목적을 가장 먼저 파악하세요.
2. CPU, 메인보드, RAM, 그래픽카드, SSD, 파워, 케이스를 포함하세요.
   별도 CPU 쿨러가 필요하면 견적에 포함하세요.
3. CPU 소켓, 메인보드 BIOS 지원, RAM 규격, 파워 용량과 커넥터,
   그래픽카드 및 쿨러의 케이스 장착 가능 여부를 확인하세요.
   확인할 수 없는 호환성은 확정하지 말고 추가 확인 사항으로 안내하세요.
4. 견적은 마크다운 표로 정리하고 예상 총액을 안내하세요.
5. 실시간 가격을 조회하지 않았다면 추정 가격임을 명시하세요.
6. 친절하고 전문적인 말투를 사용하세요.
"""

def get_api_key():
    key_path = Path(__file__).resolve().parent / "mykey.txt"
    try:
        key = key_path.read_text(encoding="utf-8-sig").strip()
    except FileNotFoundError:
        raise ValueError(f"API 키 파일을 찾을 수 없습니다.\n{key_path}\n\n이 위치에 mykey.txt를 만들어 API 키만 입력하세요.") from None
    except (OSError, UnicodeError):
        raise ValueError("mykey.txt를 읽을 수 없습니다. 파일 권한과 UTF-8 인코딩을 확인하세요.") from None
    if not key:
        raise ValueError("mykey.txt가 비어 있습니다. API 키를 입력하세요.")
    return key

## 코드 블록
from pathlib import Path
from openai import OpenAI

# 기존 코드의 설정입니다. 계정에서 이용 가능한 모델인지 확인하세요.
API_BASE_URL = "https://factchat-cloud.mindlogic.ai/v1/gateway"
MODEL_NAME = "claude-sonnet-5"

SYSTEM_PROMPT = """
당신은 20년 경력의 조립 PC 견적 전문가입니다.
1. 사용자의 예산과 주요 사용 목적을 가장 먼저 파악하세요.
2. CPU, 메인보드, RAM, 그래픽카드, SSD, 파워, 케이스를 포함하세요.
   별도 CPU 쿨러가 필요하면 견적에 포함하세요.
3. CPU 소켓, 메인보드 BIOS 지원, RAM 규격, 파워 용량과 커넥터,
   그래픽카드 및 쿨러의 케이스 장착 가능 여부를 확인하세요.
   확인할 수 없는 호환성은 확정하지 말고 추가 확인 사항으로 안내하세요.
4. 견적은 마크다운 표로 정리하고 예상 총액을 안내하세요.
5. 실시간 가격을 조회하지 않았다면 추정 가격임을 명시하세요.
6. 친절하고 전문적인 말투를 사용하세요.
"""

def get_api_key():
    key_path = Path(__file__).resolve().parent / "mykey.txt"
    try:
        key = key_path.read_text(encoding="utf-8-sig").strip()
    except FileNotFoundError:
        raise ValueError(f"API 키 파일을 찾을 수 없습니다.\n{key_path}\n\n이 위치에 mykey.txt를 만들어 API 키만 입력하세요.") from None
    except (OSError, UnicodeError):
        raise ValueError("mykey.txt를 읽을 수 없습니다. 파일 권한과 UTF-8 인코딩을 확인하세요.") from None
    if not key:
        raise ValueError("mykey.txt가 비어 있습니다. API 키를 입력하세요.")
    return key

## 실행 방법

​1단계: 이 코드는 파이썬 기본 라이브러리 외에 openai 라이브러리를 사용합니다. 명령 프롬프트(CMD)나 터미널을 열고 아래 명령어를 입력해 라이브러리를 설치합니다.(pip install openai)

2단계: 파이썬 코드 저장
사용하시는 텍스트 에디터(메모장, VS Code, 파이참 등)를 엽니다.

앞서 분리해 드린 '1. API 키 불러오기 및 기본 설정 코드'와 '2. 메인 애플리케이션 코드'를 하나의 파일에 위아래로 이어서 붙여넣습니다.

적당한 폴더(예: 바탕화면의 pc_project 폴더)를 만들고, 파일 이름을 main.py (또는 pc_quote.py 등 원하는 이름)로 저장합니다.

3단계: API 키 파일 만들기 (중요!)
코드가 정상적으로 작동하려면 API 키를 읽어올 텍스트 파일이 필요합니다.

main.py를 저장한 바로 그 동일한 폴더에 새 텍스트 파일을 만듭니다.

파일 이름을 반드시 mykey.txt로 지정합니다. (확장자가 .txt.txt가 되지 않도록 주의하세요)

mykey.txt 파일을 열고, 발급받은 API 키 문자열만 텍스트 파일에 붙여넣은 뒤 저장합니다. (줄 바꿈이나 띄어쓰기 없이 키만 입력하세요.)


4단계: 프로그램 실행
명령 프롬프트(CMD)나 터미널을 엽니다.

코드가 저장된 폴더로 이동합니다. (예: cd Desktop/pc_project)

아래 명령어를 입력하여 프로그램을 실행합니다.(python main.py)
