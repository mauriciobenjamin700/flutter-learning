# Aprendendo Flutter no Linux Ubuntu

Para configurar o `Flutter` para trabalhar em diferentes versões, usaremos o [puro](https://puro.dev/).

## Passos para Configuração

- Instalar o `puro` usando este link fornecido pela doc `curl -o- https://puro.dev/install.sh | PURO_VERSION="1.4.8" bash`
- **Caso não tenha o git** use `sudo apt install git`
- use o comando `source ~/.profile` para atualizar o PATH do seu sistema
- Feche e abra seu terminal novamente
- Use o comando `puro flutter doctor` para instalar a ultima versão estável do flutter

A baixo estão alguns comandos que o [puro](https://puro.dev/) oferece para gerenciar o flutter.

```bash
# Create a new environment "foo" with the latest stable release
puro create foo stable

# Create a new environment "bar" with with Flutter 3.13.6
puro create bar 3.13.6

# Switch "bar" to a specific Flutter version
puro upgrade bar 3.10.6

# List available environments
puro ls

# List available Flutter releases
puro releases

# Switch the current project to use "foo"
puro use foo

# Switch the global default to "bar"
puro use -g bar

# Remove puro configuration from the current project
puro clean

# Delete the "foo" environment
puro rm foo

# Run flutter commands in a specific environment
puro -e foo flutter ...
puro -e foo dart ...
puro -e foo pub ...
```

Para saber mais sobre o `puro`, clique [aqui](https://puro.dev/reference/manual/)

## Instale o Android Studio

No meu caso, bastou uma instalação padrão, mas caso eu encontre bons vídeos, vou deixa-los nesta seção

[How to Install Flutter on Ubuntu 24.04 LTS Linux | Android Studio](https://www.youtube.com/watch?v=mtqTnGAAHw0)

## Comandos Base

- Crie um projeto flutter usando `flutter create NOME_DO_SEU_PROJETO`

## Estrutura de um Projeto Flutter

Quando criamos um projeto flutter usando ``flutter create NOME_DO_SEU_PROJETO``, o flutter gera uma estrutura de arquivos e diretórios para auxiliar no desenvolvimento. Vamos discutir sobre essa estrutura

```bash
NOME_DO_SEU_PROJETO/
    .dart_tool/
    .ideia/
    android/
    ios/
    lib/
    linux/
    macos/
    test/
    web/
    widows/
    .gitignore
    .metadata
    analysis_options.yaml
    core.iml
    pubspec.lock
    pubspec.yaml
    README.md
```

Objetivo de cada pasta se resume a:

- `build/`: Todo o código gerado pelo flutter para a distribuição desejada ficará nesta pasta
- `lib/` Todo nosso código da aplicação em dart ficará nesta pasta´
- `test/`Todos os seus testes devem ficar nesta pasta.
- `pubspec.yaml` Configuração de dependências e versões do projeto.

## Executando o Projeto

Para executar seu projeto, acesse usando `cd NOME_DO_SEU_PROJETO` e use o comando `flutter run`

Se você preferir, pode executar seu projeto somente na plataforma desejada como no android studio usando `flutter run -d android`

Caso o emulador não esteja associado como padrão, você verá algo parecido com o bloco a baixo

```bash
mauriciobenjamin700@mauriciobenjamin700-Latitude-5300:~/projects/my/flutter-learning/core$ flutter run -d android
No supported devices found with name or id matching 'android'.

The following devices were found:
sdk gphone64 x86 64 (mobile) • emulator-5554 • android-x64    • Android 15 (API 35) (emulator)
Linux (desktop)              • linux         • linux-x64      • Ubuntu 24.04.1 LTS 6.8.0-51-generic
Chrome (web)                 • chrome        • web-javascript • Google Chrome 132.0.6834.159
```

use `flutter run -d emulator-5554` trocando `emulator-5554` pelo id do seu emulador que aparecer
