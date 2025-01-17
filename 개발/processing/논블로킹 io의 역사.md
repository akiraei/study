과거에 **하드웨어 장치가 독립적이지 않았던 시절**에는 오늘날의 **논블로킹 I/O**와 같은 개념은 사실상 존재하지 않았습니다. **하드웨어 장치의 발전**과 **운영 체제의 역할 강화**가 논블로킹 모델을 가능하게 했습니다. 이를 더 깊이 이해하려면, **하드웨어와 운영 체제의 발전 과정**을 살펴볼 필요가 있습니다.

---

## **1. 과거 하드웨어 장치의 한계**

초기 컴퓨터 시스템에서 하드웨어 장치는 지금처럼 독립적이지 않았습니다.

- **하드웨어 장치에 전용 프로세서가 없었음**:
    - 예를 들어, 디스크 읽기/쓰기, 네트워크 데이터 송수신 등 모든 작업은 **메인 CPU가 직접 수행**해야 했습니다.
    - CPU가 디스크에서 데이터를 읽는 동안 다른 작업을 수행할 수 없었기 때문에 **블로킹 I/O**만 가능했습니다.
- **운영 체제의 역할이 제한적**:
    - 초기 운영 체제는 장치와 CPU 사이의 중간 관리자가 아니었고, CPU가 하드웨어와 직접 상호작용.
    - CPU는 I/O 작업이 완료될 때까지 기다려야 했습니다.

---

## **2. 논블로킹 모델이 불가능했던 이유**

### **2.1 메인 CPU의 전담 작업**

- CPU가 하드웨어 장치와 직접 통신하면서 데이터를 읽고 쓰는 데 시간을 소모.
- 작업 대기 상태를 피할 수 있는 메커니즘이 부족.

### **2.2 하드웨어의 비독립성**

- 하드웨어 장치에는 처리 능력이 없었기 때문에, 모든 계산과 데이터 처리는 CPU가 담당.
- CPU가 특정 작업을 수행하는 동안, 다른 작업을 병렬로 처리할 수 없었음.

---

## **3. 하드웨어 독립성과 논블로킹 I/O의 등장**

### **3.1 하드웨어 장치의 발전**

- **컨트롤러와 전용 프로세서의 등장**:
    - 1960~70년대에 디스크 컨트롤러, 네트워크 인터페이스 컨트롤러(NIC) 등의 하드웨어가 발전.
    - 하드웨어 장치에 전용 프로세서와 메모리가 추가되면서, 독립적으로 I/O 작업을 처리할 수 있게 됨.
    - 예: 디스크 컨트롤러가 디스크 읽기/쓰기 작업을 처리하고 완료 상태만 CPU에 알림.

#### **하드웨어 독립성의 효과**

- CPU가 모든 작업을 처리하지 않아도 됨.
- 작업 분담이 가능해지면서, CPU는 다른 프로세스를 처리하는 데 집중.

---

### **3.2 운영 체제의 역할 강화**

- **운영 체제가 중재자 역할 수행**:
    - 하드웨어와 애플리케이션 간의 인터페이스를 제공.
    - OS가 I/O 요청을 관리하고, 하드웨어 장치와 메인 CPU 간의 작업을 조정.

#### **논블로킹 I/O를 위한 OS 지원**

- **논블로킹 시스템 콜**:
    - 1980년대 이후, UNIX 계열 OS에서 `select()`와 같은 시스템 콜을 제공.
    - 요청한 작업이 완료되지 않아도 CPU가 대기하지 않고 다음 작업으로 넘어갈 수 있는 기능.
- **DMA(Direct Memory Access)**:
    - CPU가 메모리에 직접 접근하지 않고, 하드웨어 장치가 데이터를 메모리로 전송.
    - CPU는 데이터 전송을 기다리지 않고 다른 작업 수행.

---

## **4. 현대 논블로킹 I/O의 작동 배경**

### **4.1 하드웨어 장치의 독립성**

- 디스크 컨트롤러, GPU, NIC 등은 각각 자체적인 프로세서를 포함.
- I/O 작업이 시작되면 CPU는 작업을 하드웨어 장치에 위임하고 대기하지 않음.

### **4.2 OS의 논블로킹 지원**

- **Epoll, Kqueue, IOCP**와 같은 고급 I/O 멀티플렉싱 메커니즘이 추가됨.
- 이러한 기능은 CPU가 한 번에 많은 I/O 요청을 관리할 수 있게 만듦.

---

## **5. 과거에서 현재까지의 I/O 모델 발전**

|**시대**|**특징**|
|---|---|
|**초기 (1950~60년대)**|CPU가 하드웨어를 직접 제어. 블로킹 I/O만 가능.|
|**컨트롤러 등장 (1970~80년대)**|디스크/네트워크 컨트롤러가 도입되어 하드웨어가 일부 작업을 독립적으로 처리. 논블로킹 개념의 시작.|
|**OS 발전 (1980~90년대)**|논블로킹 I/O 시스템 콜(`select`, `poll`) 등장. OS가 I/O 요청 관리를 중재.|
|**현대 (2000년대 이후)**|고성능 I/O 멀티플렉싱(`epoll`, `kqueue`)과 논블로킹 프레임워크(Node.js, WebFlux 등) 보편화.|

---

## **6. 결론**

**과거에는 하드웨어 장치의 독립성이 부족했기 때문에 논블로킹 I/O는 사실상 불가능**했습니다. 논블로킹 I/O의 구현은 하드웨어 장치의 발전(컨트롤러, 전용 프로세서)과 운영 체제의 개선(논블로킹 시스템 콜, DMA 등) 덕분에 가능해졌습니다.

오늘날의 논블로킹 I/O는 하드웨어와 OS의 역할 분담을 기반으로, CPU가 블로킹 없이 효율적으로 작업을 수행할 수 있도록 설계되었습니다. 😊