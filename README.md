# 프로젝트 이름:맞춤형 조립 PC 견적 추천 및 호환성 체크 챗봇 프로

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

## 코드 블록
def load_api_key(filepath="mykey.txt"):
    if not os.path.exists(filepath):
        print(f"❌ 오류: '{filepath}' 파일이 없습니다. API 키를 담은 텍스트 파일을 같은 폴더에 생성해주세요.")
        return None
        
    with open(filepath, "r", encoding="utf-8") as file:
        return file.read().strip()

# 2. LLM API 호출 함수
def chat_with_llm(user_input, chat_history, api_key):
    # API 엔드포인트 주소 (Mindlogic API 가이드에 맞춰 수정 필요)
    url = "https://api.mindlogic.ai/v1/chat/completions" 
    
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }
    
    # 시스템 프롬프트 설정 (다이어트 & 치팅데이 식단 추천 역할 부여)
    system_prompt = {
        "role": "system", 
        "content": """당신은 사용자의 식단을 관리하고 맛있는 음식을 추천해주는 '푸드 큐레이터'입니다. 
        사용자가 '다이어트'를 언급하면 칼로리가 낮고 영양가가 높은 식단을 랜덤하게 1가지 추천하고, 
        '치팅데이'를 언급하면 스트레스를 풀 수 있는 아주 맛있고 만족감 높은 속세의 음식을 1가지 추천해주세요.
        추천할 때는 다음 양식을 지켜주세요:
        1. 메뉴 이름
        2. 추천하는 이유
        3. 대략적인 영양 정보나 칼로리 (치팅데이의 경우 맛있게 먹는 팁으로 대체 가능)"""
    }
    
    # 대화 기록 구성 (문맥 유지를 위해 이전 대화 포함)
    messages = [system_prompt] + chat_history + [{"role": "user", "content": user_input}]
    
    data = {
        "model": "factchat-model-name", # 단가표에 명시된 실제 모델명으로 변경하세요 (예: factchat-3.5-turbo 등)
        "messages": messages,
        "max_tokens": 800,
        "temperature": 0.8 # 다양한 랜덤 메뉴 추천을 위해 온도를 약간 높게 설정
    }
    
    try:
        response = requests.post(url, headers=headers, data=json.dumps(data))
        response.raise_for_status() 
        result = response.json()
        
        # 3주차 계획인 '토큰 사용량 로깅'을 위한 데이터 추출 예시 (Grafana 연동용)
        # usage = result.get('usage', {})
        # print(f"[System Log] Prompt: {usage.get('prompt_tokens')}, Completion: {usage.get('completion_tokens')}")
        
        return result['choices'][0]['message']['content']
        
    except Exception as e:
        return f"API 통신 중 오류가 발생했습니다: {e}"

# 3. 메인 챗봇 실행 루프
def main():
    api_key = load_api_key("mykey.txt")
    if not api_key:
        return

    print("="*60)
    print("🥗 다이어트 & 🍕 치팅데이 맞춤 식단 추천 챗봇을 시작합니다!")
    print("   ('다이어트 메뉴 추천해줘' 또는 '오늘 치팅데이 메뉴 골라줘'라고 입력해보세요)")
    print("   (종료하시려면 '종료', 'exit', 'quit' 중 하나를 입력하세요)")
    print("="*60)
    
    chat_history = [] 
    
    while True:
        user_input = input("\n👤 당신: ")
        
        if user_input.lower() in ['종료', 'exit', 'quit']:
            print("🤖 챗봇: 대화를 종료합니다. 오늘도 맛있는 하루 보내세요!")
            break
            
        print("🤖 챗봇: (메뉴를 신중하게 고르는 중...)")
        
        # API 호출 및 답변 받기
        bot_response = chat_with_llm(user_input, chat_history, api_key)
        print(f"\n🤖 챗봇:\n{bot_response}")
        
        # 대화 기록 업데이트 
        chat_history.append({"role": "user", "content": user_input})
        chat_history.append({"role": "assistant", "content": bot_response})
        
        # 메모리가 너무 길어지는 것을 방지 (최근 6개 대화만 유지)
        if len(chat_history) > 6:
            chat_history = chat_history[-6:]

if __name__ == "__main__":
    main()

## 실행 방법

​```text
실행 방법은 프로젝트가 진행되면서 추가할 예정입니다.
​```
