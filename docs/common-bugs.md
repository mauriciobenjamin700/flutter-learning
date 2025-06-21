# 🛠️ Debugging Flutter no WSL2 (Ubuntu) com Emulador Android

Este documento registra os **principais erros encontrados** e suas **respectivas soluções**, durante a configuração e execução de projetos Flutter usando Android Emulator no WSL2.

## ✅ Estrutura do Ambiente

* **SO**: Ubuntu 24.04 rodando no WSL2
* **Flutter via `puro`**
* **SDK Android instalado em**: `/usr/lib/android-sdk`
* **Emulador via AVD Manager**
* **Variáveis configuradas em `.bashrc`**

## 📌 Erros & Soluções

---

### ❌ `flutter devices` não detecta celular físico

**Motivo:** WSL2 não detecta USB por padrão.

**Solução:**

* Testes em celulares reais não funcionam diretamente no WSL2.
* Alternativas:

  * Rodar o Flutter no Windows nativo
  * Usar emulador no Linux/WSL com suporte a KVM (veja abaixo)
  * Usar `flutter run -d web-server`

---

### ❌ Gradle Build Fails – `Could not resolve project :gradle`

**Mensagem:**

```bash
Could not resolve all task dependencies for configuration 'classpath'.
...
Incompatible because this component declares Java 11 and the consumer needed Java 8
```

**Motivo:** Versão incompatível do Java.

**Solução:**

* Use **Java 17** (ou mais recente). No seu caso, disponível em:

  ```bash
  /usr/lib/jvm/java-21-openjdk-amd64
  ```

* Atualize no `.bashrc`:

  ```bash
  export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
  export PATH=$PATH:$JAVA_HOME/bin
  ```

---

### ❌ `JAVA_HOME is set to an invalid directory`

**Solução:**

* Verifique com `ls /usr/lib/jvm/` os diretórios válidos
* Use um deles como `JAVA_HOME`, preferencialmente `java-21-openjdk-amd64`

---

### ❌ `Could not locate aapt. Please ensure you have the Android buildtools installed.`

**Motivo:** Apesar do `build-tools` estarem instalados, o Flutter não os encontrou.

**Solução:**

* Instale o pacote correto:

  ```bash
  sdkmanager "build-tools;34.0.0"
  ```

* Verifique se `aapt` está presente:

  ```bash
  ls $ANDROID_HOME/build-tools/34.0.0/aapt
  ```

---

### ❌ `Is your project missing an android/app/src/main/AndroidManifest.xml?`

**Motivo:** Flutter não encontrava a pasta `android/app`.

**Verificação:**

```bash
ls android/app/src/main/AndroidManifest.xml
```

**Solução:** O arquivo estava presente, então o erro era decorrente da falha do `aapt` acima. Resolver o `build-tools` resolveu esse erro também.

---

### ❌ `Minimum supported Gradle version is 8.4. Current version is 8.3`

**Mensagem completa:**

```bash
Failed to apply plugin 'com.android.internal.version-check'.
> Minimum supported Gradle version is 8.4. Current version is 8.3.
```

**Solução:**

1. Edite:

   ```bash
   nano android/gradle/wrapper/gradle-wrapper.properties
   ```

2. Atualize:

   ```properties
   distributionUrl=https\://services.gradle.org/distributions/gradle-8.4-all.zip
   ```

---

### ❌ `avdmanager: command not found`

**Solução:**
Instale as ferramentas de linha de comando com:

```bash
sudo apt install google-android-cmdline-tools-13.0-installer
```

**Adicione ao PATH:**

```bash
export PATH=$PATH:/usr/lib/android-sdk/cmdline-tools/latest/bin
```

---

### ❌ `Permission denied for KVM` (ao rodar emulador)

**Mensagem:**

```bash
This user doesn't have permissions to use KVM (/dev/kvm)
```

**Solução:**

```bash
sudo gpasswd -a $USER kvm
sudo chown root:kvm /dev/kvm
sudo chmod 660 /dev/kvm
```

Depois:

```bash
wsl --shutdown
```

E reabra o terminal.

---

## ✅ Dicas Extras

### 📦 Verificar SDKs Instalados

```bash
sdkmanager --list
```

### 📱 Instalar emuladores recentes

```bash
sdkmanager "system-images;android-34;google_apis;x86_64"
avdmanager create avd -n pixel_7_android_34 -k "system-images;android-34;google_apis;x86_64"
```

---

## 🧪 Comandos úteis

```bash
flutter doctor -v
flutter emulators
flutter emulators --launch <name>
flutter run -d emulator-5554
flutter clean
flutter pub get
```

---
