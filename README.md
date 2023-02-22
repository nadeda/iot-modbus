#iot-modbus

#### 소개하다
netty 프레임워크를 기반으로 하는 사물 인터넷 통신 프로토콜은 COM(직렬 포트) 및 TCP 프로토콜을 지원하고 서버 및 클라이언트의 두 가지 모드를 지원하며 스마트 장치의 Java 제어를 실현하고 장치 그룹에서 여러 장치의 높은 동시 통신을 지원합니다. . 공장 설계 모드를 채택하고 코드는 상속 및 재작성으로 고도로 캡슐화되어 SDK로 캡슐화된 인터페이스를 제공할 수 있으므로 특정 비즈니스 개발자는 통신 프로토콜의 기본 구현에 신경을 쓸 필요가 없습니다. 인터페이스를 직접 호출하여 사용할 수 있습니다. 심장 박동, 백라이트, 코드 스캔, 카드 스와이프, 손가락 정맥, 온도 및 습도, 도어록(멀티 잠금 지원), LCD 디스플레이 및 3색 알람 조명 제어의 명령 제어를 실현했습니다. 코드는 사용하기 매우 쉬운 업로드 및 전송 명령의 예를 포함하여 주석이 풍부합니다.

#### 릴리즈 노트
1. 버전 V1.0.0은 TCP 서버 통신 모드만 지원합니다.
2. 버전 V2.0.0은 TCP 서버 및 클라이언트 모드를 지원하며 클라이언트 모드는 하트비트 재연결 메커니즘도 추가합니다.
3. 버전 V3.0.0은 COM(직렬 포트) 및 TCP 프로토콜을 지원하며 파일 크기 및 시간에 따라 출력되는 로그백 로그를 추가합니다.
4. V3.1.0 버전 코드가 최적화되고 공개 모듈 하위 프로젝트가 추출됩니다.
5. V3.2.0 버전 TCP 통신은 LCD 디스플레이 제어 명령에 대한 지원을 추가하고 LCD 디스플레이의 일괄 제어를 지원합니다.
6. V3.2.1 직렬 포트 통신은 LCD 디스플레이 제어 명령에 대한 지원을 추가하고 LCD 디스플레이의 일괄 제어를 지원합니다.
7. V3.2.2 버전에서 직렬 포트 통신 수신 명령 데이터 압축 해제 처리 코드가 최적화되고 네트워크 포트 통신이 3색 경보등 제어 명령에 대한 지원을 추가합니다.
8. V3.2.3 버전의 직렬 포트 통신은 3색 경보등 제어 명령에 대한 지원을 추가하고 직렬 포트 통신은 명령 데이터 압축 해제 처리 코드 최적화를 수신합니다.
9. V3.2.4 버전은 netty를 사용하여 Rxtx를 통합하여 직렬 데이터를 압축 해제하고 지정맥 지침을 최적화합니다.
10. V3.2.5 클라이언트 모드는 명령 데이터를 송수신하기 위해 여러 서버에 대한 동시 연결을 지원합니다.
11. V3.2.6 버전은 장치 온라인, 오프라인 및 비즈니스 예외의 모니터링 및 처리를 지원합니다.
12. V3.2.7 버전은 클라이언트 모드가 연결 해제/연결 해제될 때 서버에 다시 연결하는 메커니즘을 최적화합니다.
13. V3.2.8 버전은 서버 온라인, 오프라인 모니터링 처리 및 클라이언트 하트비트 감지를 최적화합니다.
14. V3.2.9 버전은 주로 spring-boot-devtools 도구를 통합하여 핫 배포를 지원하고 개발 효율성을 개선하며 애플리케이션을 수동으로 다시 시작할 필요가 없습니다.

#### 소프트웨어 아키텍처
소프트웨어 아키텍처 설명
인프라는 Spring Boot2.x + Netty4.X + Maven3.6.x를 사용하고 로그는 logback을 사용합니다.

#### 설치 튜토리얼

1. 시스템 Windows7 이상;
2. Jdk1.8 이상을 설치합니다.
2. Maven3.6 이상을 설치합니다.
3. 코드를 Eclipse 또는 Idea에 Maven 프로젝트로 가져옵니다.

#### 사용 지침

1. 프로젝트 구조 설명:
- iot-modbus //IOT 통신 상위 프로젝트
- ├── doc //문서 관리
- ├── iot-modbus-client //netty 통신 클라이언트
- ├── iot-modbus-common //공통 모듈 하위 프로젝트
- ├── iot-modbus-netty //netty 통신 하위 프로젝트
- ├── iot-modbus-serialport //직렬 통신 하위 프로젝트
- ├── iot-modbus-server //netty 통신 서버
- ├── iot-modbus-test //샘플 하위 프로젝트 사용
- └── 도구 //통신 명령 디버깅 도구
2. 구성 파일 iot-modbus-test 하위 프로젝트의 리소스 디렉터리 아래에 있는 application.yml 파일을 확인합니다.
3. 파일을 시작하여 iot-modbus-test 하위 프로젝트 App.java 파일을 봅니다.
4. 서비스 시작 후 기본 서버 포트: 8080, 기본 네트워크 포트 통신 포트: 4000, 직렬 통신용 기본 직렬 포트: COM1;
5. 통신 명령 디버깅 도구, TCP 통신 모드의 경우 도구 디렉토리에서 NetAssist.exe를 사용하고 직렬 통신 모드의 경우 도구 디렉토리의 UartAssist.exe를 사용하십시오.
6. 통신 명령은 Hex 인코딩(16진수)을 채택합니다.
7. 직렬 통신 종속 파일의 경우 doc/serialport 디렉터리를 확인하고 rxtxParallel.dll 및 rxtxSerial.dll 파일을 Windows 환경에서 JDK 설치의 bin 디렉터리에 복사하고 librxtxParallel.so 및 librxtxSerial.so 파일을 Linux 환경에서 JDK 설치의 bin 디렉토리 다운;
8. 도어록(단일잠금/다중잠금), 코드스캐닝, 백라이트, LCD 디스플레이, 삼색 등 우편 배달부 명령 전달 샘플은 doc/postman/communication 명령 delivery.postman_collection.json 파일을 참조하세요. 경보등 명령 .

#### 명령 형식

1. 하트비트 명령(7E 04 00 BE 01 00 00 74 77 7F)을 예로 들면 아래 첨자는 0부터 시작합니다.
2. 0번째 비트는 시작 문자이고 길이는 1바이트를 차지하도록 고정되며 고정 형식은 7E입니다.
3. 첫 번째와 두 번째 숫자는 데이터 길이이며, 계산 방법은 명령 문자에서 데이터 비트까지(예: 3자리에서 명령 길이 - 3자리) 길이는 2바이트를 차지하도록 고정됩니다. : 04 00, 길이가 4임을 나타냅니다.
4. 세 번째 비트는 명령 기호이고 길이는 1바이트를 차지하도록 고정됩니다. 예: BE는 하트비트 명령을 의미합니다.
5. 네 번째 비트는 장치 번호이고 길이는 1바이트를 차지하도록 고정됩니다. 예: 01은 장치 번호가 1임을 나타냅니다.
6. 다섯 번째 비트는 레이어 주소이고 길이는 1바이트로 고정됩니다. 예를 들어 00은 장치의 모든 레이어가 실행되지 않음을 의미합니다.
7. 여섯 번째 비트는 슬롯 주소이고 길이는 1바이트를 차지하도록 고정됩니다. 예: 00, 이는 장치의 모든 슬롯이 실행되지 않음을 의미합니다.
8. 명령 길이 -3자리에서 -2자리는 검사 숫자이며 CRC16_MODBUS(길이, 명령, 주소, 데이터)를 사용하여 확인합니다. 예: 74 77, 자세한 내용은 ModbusCrc16Utils.java 도구 클래스를 참조하십시오.
9. 마지막 비트는 터미네이터이고, 길이는 1바이트로 고정되며, 고정 포맷은 7F입니다.

#### 통신 명령어

1. 하트비트 업로드 명령
- iot-modbus는 하트비트를 통해 통신을 유지하기 위해 서버로 사용됩니다.서버를 시작한 후 NetAssist.exe 명령 디버깅 도구를 열고 먼저 하트비트 명령을 서버에 보냅니다.
- 하드웨어는 서버로 전송합니다: 7E 04 00 BE 01 00 00 74 77 7F, 이는 필수 명령입니다.
2. 백라이트 업로드 명령
- 하드웨어가 서버로 전송: 7E 05 00 88 01 00 00 00 AF E3 7F
3. 코드 스캔 명령 보내기
- 서버가 하드웨어로 전송: 7E 05 00 08 01 00 00 01 6F FD 7F
- 7번째 비트는 데이터 비트이고 길이는 1바이트를 차지하도록 고정됩니다. 예: 01은 도크 스캐닝을 연다는 의미입니다.
4. 업로드할 스캔 코드 명령
- 하드웨어가 서버로 전송: 7E 24 00 8F 01 00 00 03 45 30 30 34 30 31 30 38 32 38 30 32 41 36 39 33 0D 02 00 00 01 02 13 73 02 00 00 01 02 7F 993
- 데이터 비트: 03 45 30 30 34 30 31 30 38 32 38 30 32 41 36 39 33 0D 02 00 00 01 02 13 73 02 00 00 01 02 13 73은 바코드 정보입니다.
5. 카드 스 와이프 지침 업로드
- 하드웨어가 서버로 전송: 7E 08 00 84 01 00 00 86 14 AE 02 7C 53 7F
- 데이터 비트: 86 14 AE 02는 카드 번호 정보입니다.
6. 단일 잠금 해제 명령 발행
- 서버가 하드웨어로 전송: 7E 05 00 03 01 00 00 01 CA 3C 7F
- 7번째 비트는 데이터 비트이고 길이는 1바이트를 차지하도록 고정됩니다. 예: 01은 1번 잠금 해제를 의미합니다.
7. 여러 잠금 해제 명령 발행
- 서버가 하드웨어로 전송: 7E 08 00 03 FF FF FF 01 00 02 01 7F B0 7F
- FF FF FF는 명령어에 대한 호환 가능한 패딩 비트이며, 01 00 02 01은 데이터 비트이며 여기서 01은 잠금 번호 1, 00은 잠금, 02는 잠금 번호 2, 01은 잠금 해제를 의미합니다.
8. 잠금 상태 업로드 명령
- 하드웨어가 서버로 전송: 7E 0D 00 83 01 00 00 FF FF FF 01 00 05 02 00 01 EE 99 7F
- FF FF FF는 명령에 대한 호환 가능한 채움 비트이며, 01 00 05 02 00 01은 데이터 비트입니다. 2, 00은 잠금 잠금을 의미하고 01은 센서 상태 코드를 의미합니다.
9. 손가락 정맥 및 온도 및 습도 지침(설명 없음, 자세한 내용은 코드를 확인하십시오)
10. LCD 디스플레이 배치 제어 및 발행 명령(설명 없음, 자세한 내용은 코드를 확인하십시오);
11. 3색 경보등 제어 및 발행 지침(설명 없음, 자세한 내용은 코드를 확인하십시오).

#### 디버그 지침

1. 아래 그림과 같이 iot-modbus-test 하위 프로젝트 App.java 파일을 찾아 서버를 시작합니다.
- ![사진 설명 입력](doc/picture/1%E9%A1%B9%E7%9B%AE%E5%90%AF%E5%8A%A8.png)
- 참고: 프로젝트가 성공적으로 시작된 후 콘솔 로그 출력 서버의 포트: 8080, 프로젝트 서비스 이름: iot-modbus-test, 서버에서 열린 소켓 통신 포트: 4000.
2. 프로젝트 도구 디렉토리 아래의 통신 명령 디버깅 도구 NetAssist를 Windows 데스크탑에 복사하고 두 번 클릭하여 연 다음 아래 그림과 같이 매개변수를 구성합니다.
- ![사진 설명 입력](doc/picture/2NetAssist%E5%8F%82%E6%95%B0%E9%85%8D%E7%BD%AE.png)
- 참고: 프로토콜 유형은 TCP 클라이언트 선택(디버깅 도구는 하드웨어 통신 시뮬레이션을 위한 고객 서비스 터미널로 사용), 원격 호스트 주소는 로컬 컴퓨터의 IP 주소 입력, 원격 호스트는 서버 포트 4000 입력 포트, 수신 및 송신 인코딩에 대해 HEX를 선택하고 마지막으로 연결 버튼을 클릭하여 연결합니다.연결이 성공한 후 서버 콘솔의 로그 출력은 아래 그림과 같습니다.
- ![사진 설명 입력](doc/picture/3%E8%BF%9E%E6%8E%A5%E6%88%90%E5%8A%9F.png)
3. 클라이언트는 아래 그림과 같이 하트비트 명령을 서버에 업로드합니다.
- ![사진 설명 입력](doc/picture/4%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BF%83%E8%B7%B3%E6%8C%87% E4%BB%A4%E4%B8%8A%E4%BC%A0.png)
- 설명 : 하트비트 명령어를 통신 명령어 디버깅 도구인 NetAssist의 데이터 전송 창에 복사하여 붙여넣기 한 후 보내기 버튼을 클릭하면 아래 그림과 같이 서버에서 하트비트 명령어를 수신하게 됩니다.
- ![사진 설명 입력](doc/picture/5%E6%9C%8D%E5%8A%A1%E7%AB%AF%E6%8E%A5%E6%94%B6%E5%88%B0% E5%BF%83%E8%B7%B3%E6%8C%87%E4%BB%A4.png)
- 참고: 클라이언트에서 서버로 하트비트 명령을 일정한 간격으로 전송하여 고객 서비스와 서버 간의 통신 연결을 유지해야 합니다. 프로덕션 환경에서 전송 빈도는 일반적으로 5초에 한 번으로 설정할 수 있습니다. 통신 연결이 끊어지면 고객 서비스와 서버가 통신할 수 없습니다. 참고: 디버깅 프로세스 중에 통신 명령 디버깅 도구 NetAssist가 서버에서 연결 해제된 경우 NetAssist 연결 버튼을 수동으로 클릭하여 서버에 하트비트 명령을 다시 보내야 합니다.
4. 아래 그림과 같이 Postman을 사용하여 클라이언트에 제어 단일 잠금 명령을 발행하도록 서버에 요청하십시오.
- ![사진 설명 입력](doc/picture/6postman%E8%AF%B7%E6%B1%82%E4%B8%8B%E5%8F%91%E6%8E%A7%E5%88%B6% E5%8D%95%E9%94%81%E6%8C%87%E4%BB%A4.png)
- 설명: Postman에서 서버의 인터페이스를 입력하여 제어 단일 잠금 명령을 보내고 요청 주소와 매개변수를 입력합니다. http://127.0.0.1:8080/iot-modbus-test/test/openlock/1/ 1, 서버 콘솔 로그 출력은 아래 그림과 같습니다.
- ![사진 설명 입력](doc/picture/7%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8B%E5%8F%91%E6%8E%A7% E5%88%B6%E5%8D%95%E9%94%81%E6%8C%87%E4%BB%A4.png)
- 고객 서비스 측의 명령 디버깅 도구 NetAssist는 아래 그림과 같이 제어 단일 잠금 명령을 수신합니다.
- ![사진 설명 입력](doc/picture/8%E6%9C%8D%E5%8A%A1%E7%AB%AF%E6%8E%A5%E6%94%B6%E5%88%B0% E6%8E%A7%E5%88%B6%E5%8D%95%E9%94%81%E6%8C%87%E4%BB%A4.png)
5. 기타 업로드 및 전송 방법은 자세하게 소개하지 않으며, 관심 있는 학생들은 프로젝트 디렉토리의 doc/postman 디렉토리에 있는 통신 방법 및 요청 샘플을 참조할 수 있습니다.

#### 창조는 쉽지 않습니다. 별을 밝히는 것을 잊지 마세요. 여러분의 지원은 제 끊임없는 동기입니다.

#### 교환 그룹 가입을 환영합니다

- 위챗 공개 계정
- ![사진 설명 입력](doc/picture/TF%EF%BC%88%E8%85%BE%E9%A3%9E%EF%BC%89%E5%BC%80%E6%BA%90% E5%85%AC%E4%BC%97%E5%8F%B7%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg)
- QQ 기술교류단
- ![사진 설명 입력](doc/picture/9%E7%89%A9%E8%81%94%E7%BD%91%E9%80%9A%E8%AE%AF%E5%8D%8F% E8%AE%AE%EF%BC%88iot-modbus%EF%BC%89%E4%BA%A4%E6%B5%81%E7%BE%A4%E7%BE%A4%E4%BA%8C% E7%BB%B4%E7%A0%81.png)
