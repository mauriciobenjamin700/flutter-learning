# Aprendendo a Configurar o Flutter no Linux Ubuntu

Para configurar o `Flutter` para trabalhar em diferentes versões, usaremos o [puro](https://puro.dev/).

## Passos

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
