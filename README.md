# 프로젝트 이름:맞춤형 조립 PC 견적 추천 및 호환성 체크 챗봇 프로젝

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
추가예정입니다.

# 1. API 키 불러오기
def get_api_key(filepath="mykey.txt"):
    """mykey.txt 파일에서 API 키를 읽어오는 함수"""
    try:
        with open(filepath, "r", encoding="utf-8") as file:
            return file.read().strip()
    except FileNotFoundError:
        print(f"오류: '{filepath}' 파일을 찾을 수 없습니다. 같은 폴더에 키 파일을 준비해주세요.")
        return None

## 코드 블록
from openai import OpenAI
import os

# 1. 외부 함수(또는 위에서 선언한 함수)를 통해 API 키 불러오기
api_key = get_api_key("mykey.txt")

if api_key:
    # 2. FactChat API 클라이언트 설정 (OpenAI 호환)
    client = OpenAI(
        api_key=api_key,
        base_url="https://factchat-cloud.mindlogic.ai/v1/gateway"
    )

    print("==================================================")
    print(" 🛠️  맞춤형 PC 견적 전문가 챗봇이 시작되었습니다! ")
    print(" (종료를 원하시면 '종료', 'exit', 'quit'를 입력하세요)")
    print("==================================================\n")
    
    # 3. 챗봇 페르소나 부여 (System Prompt)
    system_prompt = """
    당신은 20년 경력의 조립 PC 견적 전문가입니다. 다음 원칙을 엄격하게 지켜 답변해주세요:
    1. 사용자의 '예산'과 '주요 사용 목적'을 가장 먼저 파악하세요.
    2. 부품 추천 시 CPU, 메인보드, RAM, 그래픽카드(VGA), SSD, 파워, 케이스를 포함해야 합니다.
    3. CPU 소켓과 메인보드의 호환성, 그래픽카드와 파워 용량의 호환성을 반드시 체크하고 설명해주세요.
    4. 견적을 제안할 때는 보기 쉽게 마크다운 표(Markdown Table) 형식으로 정리해서 보여주세요.
    5. 친절하지만 전문적인 말투(예: ~입니다, ~을 권장합니다)를 사용하세요.
    """

    messages = [{"role": "system", "content": system_prompt}]

    # 4. 대화 루프 실행
    while True:
        user_input = input("👤 사용자: ")
        
        if user_input.lower() in ['종료', 'exit', 'quit']:
            print("👋 챗봇을 종료합니다.")
            break
            
        messages.append({"role": "user", "content": user_input})
        
        try:
            print("🤖 챗봇이 견적을 구성하는 중입니다...\n")
            
            # API 호출 (모델명은 claude-sonnet-5 사용)
            response = client.chat.completions.create(
                model="claude-sonnet-5", 
                messages=messages
            )
            
            bot_reply = response.choices[0].message.content
            print(f"🛠️ 전문가:\n{bot_reply}\n")
            print("-" * 50)
            
            # 대화 기록 유지
            messages.append({"role": "assistant", "content": bot_reply})
            
        except Exception as e:
            print(f"⚠️ API 호출 중 오류가 발생했습니다: {e}")

## 실행 방법

​```text
실행 방법은 프로젝트가 진행되면서 추가할 예정입니다.
​```
