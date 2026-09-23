# Manual — Meu Files pessoal no elementary OS

## Objetivo

Este manual descreve como recuperar e reinstalar minha versão pessoal do elementary Files depois de uma reinstalação do elementary OS.

O código está no meu fork:

`https://github.com/myhousece/elementary-files-pr`

A branch pessoal é:

`meu-files`

---

## Estrutura do projeto

Repositório oficial:

`https://github.com/elementary/files.git`

Meu fork:

`https://github.com/myhousece/elementary-files-pr.git`

Branch pessoal:

`meu-files`

Remote oficial:

`upstream`

Remote do meu fork:

`origin`

---

## Instalação pessoal

O Files oficial do elementary é instalado pelo pacote:

`pantheon-files`

O Files oficial utiliza:

`/usr/bin/io.elementary.files`

Minha versão pessoal é instalada em:

`~/.local/bin/io.elementary.files`

A instalação pessoal deve permanecer em `~/.local` para não substituir diretamente os arquivos do pacote oficial.

---

## Depois de reinstalar o elementary OS

### 1. Criar a pasta de projetos

```bash
mkdir -p ~/Projetos
```

### 2. Clonar meu fork

```bash
git clone https://github.com/myhousece/elementary-files-pr.git ~/Projetos/files
```

### 3. Entrar no projeto

```bash
cd ~/Projetos/files
```

### 4. Selecionar minha branch

```bash
git switch meu-files
```

### 5. Verificar o estado

```bash
git status
```

O esperado é:

```text
On branch meu-files
```

---

## Configurar o remote oficial

Adicionar o repositório oficial como `upstream`:

```bash
git remote add upstream https://github.com/elementary/files.git
```

Verificar:

```bash
git remote -v
```

Deve aparecer:

```text
origin    https://github.com/myhousece/elementary-files-pr.git
upstream  https://github.com/elementary/files.git
```

Se `upstream` já existir, não executar novamente o comando `git remote add`.

---

## Configurar o build

Dentro de:

`~/Projetos/files`

executar:

```bash
meson setup build-personal --prefix="$HOME/.local"
```

O `--prefix="$HOME/.local"` é importante porque mantém a instalação da versão pessoal dentro da pasta do usuário.

Não instalar esta versão diretamente em `/usr`.

---

## Compilar

```bash
meson compile -C build-personal
```

A compilação deve terminar sem erros.

---

## Instalar

```bash
meson install -C build-personal
```

Não usar `sudo`.

A instalação será feita em:

`~/.local`

---

## Testar

Executar:

```bash
~/.local/bin/io.elementary.files
```

Verificar:

- tradução para português;
- menus de contexto;
- opção Comprimir;
- etiquetas/pontos coloridos;
- controles de visualização;
- posição dos controles;
- aparência das headerbars;
- comportamento do painel principal.

---

# Minhas alterações

A branch `meu-files` contém quatro commits pessoais:

```text
40cbffa3f  Troca posição dos controles de visualização
e9d9a57d3  Move view switcher to right side
b950120d5  Align Files headerbars with Code
c613d1d8a  Allow main content pane to shrink
```

Também foi feita uma alteração em:

`src/View/Window.vala`

A linha correta é:

```vala
lside_pane.pack2 (content_box, true, false);
```

em vez de:

```vala
lside_pane.pack2 (content_box, true, true);
```

Essa alteração deve ser preservada.

`src/View/Miller.vala` não deve ser alterado para reproduzir essa mudança.

---

# Atualização futura

Quando o elementary modificar o Files oficial, primeiro atualizar as informações do repositório oficial:

```bash
cd ~/Projetos/files
git fetch upstream
```

Verificar as diferenças antes de fazer qualquer integração:

```bash
git log --oneline --left-right --graph upstream/main...meu-files
```

Não fazer merge ou rebase automaticamente.

Primeiro verificar quais alterações entraram no `upstream/main` e depois decidir como incorporá-las preservando minhas alterações pessoais.

---

# Regra de instalação

Não usar:

```bash
sudo ninja install
```

nem:

```bash
sudo meson install
```

A instalação pessoal deve permanecer em:

`~/.local`

O pacote oficial do elementary continua sendo gerenciado normalmente pelo sistema.

---

# Recuperação após reinstalação

O código e os commits pessoais ficam preservados no GitHub.

Depois de uma reinstalação do elementary OS:

1. instalar as ferramentas/dependências necessárias;
2. clonar meu fork;
3. entrar na branch `meu-files`;
4. configurar o Meson com `--prefix="$HOME/.local"`;
5. compilar;
6. instalar em `~/.local`;
7. testar o Files.

Não é necessário recriar manualmente minhas alterações: elas estão registradas nos commits da branch `meu-files`.
