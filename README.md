# 🌀 Auto Infra Resilience Engine (AIRE)

![Java](https://img.shields.io/badge/Java-21-blue?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-4.0.3-brightgreen?style=for-the-badge&logo=springboot)
![AI](https://img.shields.io/badge/AI-Deterministic_Reasoning-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

## 📖 Project Description
The **Auto Infra Resilience Engine (AIRE)** is a high-concurrency traffic-shaping kernel designed to protect distributed systems from catastrophic resource exhaustion. By bridging the gap between traditional rule-based safety gates and high-level **Deterministic AI Reasoning**, AIRE creates a self-healing control plane for cloud-native applications.

> This project was originally conceived and developed under the working title **Trishul-Flux**. You will find references to this name and its internal component "Prongs" within the package structures (e.g., `io.trishul.flux`) and configuration logs. The current working / appropriate name is **Auto Infra Resilience Engine (AIRE)**.


## 🏗️ System Architecture
The system architecture follows a tri-layered approach: a **Perception Layer** that scans system metrics, a **Cognition Layer** that evaluates those metrics via an LLM, and an **Action Layer** that modifies the infrastructure parameters in real-time.

       __________________________________________________________
      |                 AUTO INFRA RESILIENCE ENGINE             |
      |__________________________________________________________|
                   |                                |
      _____________V_____________      _____________V_____________
     |      PERCEPTION LAYER     |    |      COGNITION LAYER      |
     | (Telemetry & Monitoring)  |    |    (Decision Engine)      |
     |___________________________|    |___________________________|
     | - CPU/Memory Scanning     |    | - Gemini 2.5 Flash (0.0T) |
     | - Thread Contention       |    | - Trend Analysis          |
     | - Drop Rate Tracking      |    | - Deterministic Logic     |
     |___________________________|    |___________________________|
                   |                                |
      _____________V________________________________V_____________
     |                       ACTION LAYER                         |
     |                  (Resilience Orchestrator)                 |
     |____________________________________________________________|
     |  [SCALE]  <-->  [THROTTLE]  <-->  [DRAIN]  <-->  [REBOOT]  |
     |____________________________________________________________|


## 🚦 Execution Flow
The execution flow is a closed-loop cycle. A TrafficSimulator generates load that is gated by a MockLoadBalancer. Meanwhile, a TelemetryScanner captures snapshots and stores them in a sliding-window repository. If a performance degradation is detected, the DecisionEngine consults the AI to derive a new ActionPlan, which the Orchestrator applies to the live gates.

     [ Traffic Generator ]
          |
          |   (Dynamic Bursts ~800+ RPS)
          V
     +-----------------------+       +-------------------------+
     |   Mock Load Balancer  |<------|  Resilience Orchestrator |
     +-----------+-----------+       +------------^------------+
             |                                |
        [ CHECK LIMITS ]                 [ APPLY ACTION ]
             |                                |
             V                                |
     +-----------------------+       +-------------------------+
     |      Flux Limiter     |       |     Decision Engine     |
     |    (Token Bucket)     |       |   (AI Reasoning Layer)  |
     +-----------+-----------+       +------------^------------+
             |                                |
        [ CAPTURE DATA ]                 [ ANALYZE TREND ]
             |                                |
             V                                |
     +-----------------------+       +------------+------------+
     |   Telemetry Scanner   |------>|   Telemetry Repository  |
     |  (5s Scheduled Loop)  |       |   (Sliding Window [10]) |
     +-----------------------+       +-------------------------+

## 🛠️ Tech Stack
- Language: Java 21 - Utilizing Virtual Threads for non-blocking I/O.

- Framework: Spring Boot 4.0.3.

- AI Engine: Deterministic Reasoning via Gemini 2.5 Flash / Ollama - phi3-mini.

- Monitoring: Micrometer & Spring Actuator - Custom trishul.limiter metrics.

- Concurrency: AtomicLong and LinkedBlockingDeque for thread-safe telemetry windows.

---

## 📂 Project Directory Structure

    AIRE/ 
      ├── src/main/java/io/trishul/flux/
      │   ├── TrishulFLuxApplication.java
      │   ├── agent/                 # COGNITION: AI Reasoning Layer
      │   │   ├── DecisionEngine.java     
      │   │   ├── GeminiModelClient.java / ModelClient.java 
      │   │   ├── ActionPlan.java  
      │   │   └── ResponseInterpreter.java
      │   ├── core/                  # PERCEPTION: Resilience Kernel
      │   │   ├── ratelimiter/            
      │   │   ├── telemetry/  
      │   │   └── circuitbreaker/        
      │   ├── orchestrator/          # ACTION: Execution Plane
      │   │   └── ResilienceOrchestrator.java 
      │   ├── infra/                 # ENVIRONMENT: Traffic Simulation
      │   │   ├── MockLoadBalancer.java 
      │   │   ├── HeartbeatSimulator.java 
      │   │   └── TrafficSimulator.java  
      │   └── config/                # SYSTEM: Scheduling & Pool Configuration
      ├── pom.xml                    
      └── mvnw             

## 🛡️ Robustness & Fail-Safes
AIRE is engineered with a Zero-Trust Fail-Safe. We operate under the assumption that cloud dependencies may fail.

  - Emergency Protocol: If the system stays in a CRITICAL status for 3+ cycles, the engine escalates to a deterministic escalation prompt.

  - Hard Fallback: If the Gemini API is unreachable or times out, the client automatically returns a THROTTLE command, ensuring the system fails "shut" to protect the infrastructure.

---

## 📈 Performance Benchmarks
During catastrophic stress testing simulating bursts of 800 to 1000+ RPS:

  * Availability: 100% - Zero kernel panics or resource exhaustion.

  * Recovery: Successfully returned to a HEALTHY state within 2 cycles (10 seconds) of traffic subsidence.

  * Efficiency: Virtual thread utilization kept memory overhead significantly lower than traditional thread-per-request models.
