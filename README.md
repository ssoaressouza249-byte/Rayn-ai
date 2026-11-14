name: Gerar APK Rayn AI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Extrair e corrigir URL da API
        run: |
          unzip -o rayn-ai.zip
          if [ -d "rayn-ai" ]; then cp -rn rayn-ai/* . && cp -rn rayn-ai/.* . 2>/dev/null || true; fi
          sed -i 's|https://googleapis.com|https://googleapis.com|g' app/src/main/java/com/example/network/GeminiApiClient.kt
      - name: Configurar Java 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
      - name: Configurar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '9.3.1'
      - name: Gerar APK
        run: gradle assembleDebug
      - name: Liberar Download do APK
        uses: actions/upload-artifact@v4
        with:
          name: RAYN-AI-INSTALAVEL
          path: app/build/outputs/apk/debug/app-debug.apk
          
