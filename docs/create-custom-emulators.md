# Configurando CLI

Instale a **versão mais recente disponível** do `cmdline-tools`, que no seu caso parece ser a **13.0** (caso apareça na lista).

## 🧱 1. Instale o pacote cmdline-tools

Execute:

```bash
sudo apt install google-android-cmdline-tools-13.0-installer
```

Se der erro, use a 12.0 ou 11.0:

```bash
sudo apt install google-android-cmdline-tools-12.0-installer
```

### ✅ 2. Depois de instalar, adicione ao seu PATH

Veja onde o executável foi instalado:

```bash
dpkg -L google-android-cmdline-tools-13.0-installer | grep bin
```

Você verá algo como:

```bash
/usr/lib/android-sdk/cmdline-tools/latest/bin/sdkmanager
```

Então adicione ao seu `~/.bashrc` ou `~/.zshrc`:

```bash
export ANDROID_HOME=/usr/lib/android-sdk
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
```

E ative com:

```bash
source ~/.bashrc
```

Dê permissão de escrita

```bash
sudo chown -R $USER:$USER /usr/lib/android-sdk
```

### ✅ 3. Verifique se funcionou

```bash
sdkmanager --list
avdmanager list device
```

### 🚀 4. Pronto para criar emuladores

Agora você pode seguir os passos anteriores para instalar imagens e criar emuladores customizados com:

```bash
sdkmanager "system-images;android-34;google_apis;x86_64"
avdmanager create avd -n pixel8_api34 -k "system-images;android-34;google_apis;x86_64" -d pixel_8
```
