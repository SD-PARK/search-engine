# ── 1단계: Gradle 빌드 ──────────────────────────────────────────────────
FROM mcr.microsoft.com/openjdk/jdk:21-ubuntu AS builder

WORKDIR /workspace

COPY gradlew gradlew
COPY gradle gradle/
COPY build.gradle settings.gradle ./

RUN chmod +x gradlew
RUN ./gradlew dependencies --no-daemon

# 소스 복사 후 WAR 빌드 (-x test 로 테스트 스킵)
COPY ../../src src/
RUN ./gradlew war -x test --no-daemon


# ── 2단계: Tomcat 실행 ──────────────────────────────────────────────────
FROM tomcat:10.1-jdk21-temurin AS runtime

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=builder /workspace/build/libs/search-engine.war /usr/local/tomcat/webapps/ROOT.war

# 타임존 설정
ENV TZ=Asia/Seoul

EXPOSE 8080

CMD ["catalina.sh", "run"]