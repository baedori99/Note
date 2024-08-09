---
title: Kotlin
create_date: 2024-08-09 12:08 - 2024-08-09 12:08
draft: true
---
이번에 안드로이드 앱 프로젝트를 하게 됬고, 안드로이드 앱이라 `Kotlin` 언어로 하게 되어 `Kotlin`문법에 대한 정리를 하고자 노트를 만들었다.  

## 1. Kotlin Definition

`Kotlin(코틀린)`은 IntelliJ, PyCharm 등 다양한 IDE를 선보인 것으로 유명한 `JetBrains`사에서 오픈소스 그룹을 만들어 개발한 프로그래밍 언어이다.  

자바와 100% 호환이 되며 자바보다 더 간결하고 많은 기능을 추가한 언어이다.  

`Kotlin`이 `Java`를 대체할 수 있는 이유는 **JVM 기반 언어**이기 때문이다.  JVM 기반 언어를 다르게 얘기하면, 어떠한 언어도 `자바 바이트 코드(.class)`로 컴파일할 수 있다면 동작할 수 있는 것이다.  

>[!tip] JVM이란?
>**JVM (Java Virtual Machine)**: 자바 프로그램을 실행하기 위한 실행 환경을 만들어 주는 소프트웨어이다.
>- 다른 말로 OS에 종속받지 않고 CPU가 Java를 인식, 실행할 수 있게 하는 가상 컴퓨터이다.  
>
>
>**JVM은 운영체제에 종속적이지 않다.**   
>- JVM 기반 프로그램을 `Linux`, `Windows`, `macOS`등 **어디에서나 실행 가능하다**라는 뜻이다.  

### Kotlin 언어의 특징

- 표현력과 간결함(Expressive & Concise)
	- 매우 간결한 문법을 제공한다.  
- 안전한 코드(Safer Code)
	- `Kotlin`에서는 변수 선언시 `Null` 허용과 불허용을 구분하여 선언할 수 있다.  
- 상호 운용성(Interoperable)
	-  Java와 100% 호환이 된다.  
- 구조화 동시성(Structed Concurrency)
	- 코루틴(Corutines) 기법을 이용하면 비동기 프로그래밍을 간소화할 수 있다.  

>[!tip] 코루틴(Corutine)이란?
>