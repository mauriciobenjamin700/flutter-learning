# Guia Completo: Fundamentos do Flutter com Dart

## Índice
1. [Conceitos Fundamentais do Dart](#conceitos-fundamentais-do-dart)
2. [Introdução ao Flutter](#introdução-ao-flutter)
3. [Widgets: O Coração do Flutter](#widgets-o-coração-do-flutter)
4. [StatelessWidget vs StatefulWidget](#statelesswidget-vs-statefulwidget)
5. [Layout e Posicionamento](#layout-e-posicionamento)
6. [Navegação entre Telas](#navegação-entre-telas)
7. [Gerenciamento de Estado](#gerenciamento-de-estado)
8. [Trabalhando com Listas](#trabalhando-com-listas)
9. [Formulários e Entrada de Dados](#formulários-e-entrada-de-dados)
10. [Recursos Essenciais](#recursos-essenciais)
11. [Estrutura de Projeto](#estrutura-de-projeto)

---

## Conceitos Fundamentais do Dart

### Variáveis e Tipos
```dart
// Declaração explícita de tipo
String nome = 'João';
int idade = 25;
double altura = 1.75;
bool ativo = true;

// Inferência de tipo (var)
var cidade = 'São Paulo'; // String inferido
var numero = 42; // int inferido

// Variáveis que podem ser nulas
String? email; // Pode ser null
int? telefone = null;

// Constantes
const String appName = 'Meu App'; // Compile-time constant
final DateTime agora = DateTime.now(); // Runtime constant
```

### Funções
```dart
// Função básica
String saudar(String nome) {
  return 'Olá, $nome!';
}

// Função com arrow syntax
String saudarCurto(String nome) => 'Olá, $nome!';

// Parâmetros opcionais nomeados
void configurarUsuario({required String nome, int? idade, String email = ''}) {
  // implementação
}

// Parâmetros opcionais posicionais
String formatarNome(String primeiro, [String? ultimo]) {
  return ultimo != null ? '$primeiro $ultimo' : primeiro;
}
```

### Classes e Objetos
```dart
class Pessoa {
  // Propriedades
  String nome;
  int idade;
  String? email;
  
  // Construtor
  Pessoa({required this.nome, required this.idade, this.email});
  
  // Construtor nomeado
  Pessoa.vazio() : nome = '', idade = 0;
  
  // Métodos
  void apresentar() {
    print('Sou $nome, tenho $idade anos');
  }
  
  // Getter
  bool get ehMaiorIdade => idade >= 18;
  
  // Setter
  set novaIdade(int valor) {
    if (valor >= 0) idade = valor;
  }
}

// Uso da classe
final pessoa = Pessoa(nome: 'Maria', idade: 30);
pessoa.apresentar();
```

### Listas e Mapas
```dart
// Listas
List<String> frutas = ['maçã', 'banana', 'laranja'];
List<int> numeros = [1, 2, 3, 4, 5];

// Operações com listas
frutas.add('uva');
frutas.remove('banana');
String primeira = frutas.first;
int tamanho = frutas.length;

// Mapas
Map<String, int> idades = {
  'João': 25,
  'Maria': 30,
  'Pedro': 22
};

// Acessando valores
int idadeJoao = idades['João'] ?? 0;
idades['Ana'] = 28; // Adicionar novo item
```

---

## Introdução ao Flutter

### O que é Flutter?
Flutter é um framework de desenvolvimento mobile criado pelo Google que permite criar aplicativos nativos para iOS e Android usando uma única base de código em Dart.

### Princípios Fundamentais

**Tudo é Widget**: No Flutter, absolutamente tudo na interface é um widget - textos, botões, layouts, até mesmo o próprio aplicativo.

**Composição sobre Herança**: Ao invés de herdar características, você compõe widgets menores para criar interfaces complexas.

**Declarativo**: Você descreve como a interface deve parecer em qualquer momento, não como ela deve mudar.

### Estrutura Básica de um App
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MeuApp());
}

class MeuApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Meu Primeiro App',
      home: TelaPrincipal(),
    );
  }
}

class TelaPrincipal extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Tela Principal'),
      ),
      body: Center(
        child: Text('Olá, Flutter!'),
      ),
    );
  }
}
```

---

## Widgets: O Coração do Flutter

### Widgets Básicos

**Text**: Exibe texto
```dart
Text(
  'Olá, mundo!',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
  ),
)
```

**Container**: Um widget versátil para layout e decoração
```dart
Container(
  width: 200,
  height: 100,
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.symmetric(vertical: 8),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(10),
    boxShadow: [
      BoxShadow(
        color: Colors.grey,
        blurRadius: 5,
        offset: Offset(2, 2),
      ),
    ],
  ),
  child: Text('Conteúdo'),
)
```

**Image**: Exibe imagens
```dart
// Imagem da internet
Image.network('https://exemplo.com/imagem.jpg')

// Imagem dos assets
Image.asset('assets/images/logo.png')

// Com configurações
Image.network(
  'https://exemplo.com/imagem.jpg',
  width: 150,
  height: 150,
  fit: BoxFit.cover,
)
```

**Icon**: Exibe ícones
```dart
Icon(
  Icons.favorite,
  color: Colors.red,
  size: 30,
)
```

### Widgets de Interação

**ElevatedButton**: Botão elevado
```dart
ElevatedButton(
  onPressed: () {
    print('Botão pressionado!');
  },
  child: Text('Clique aqui'),
)
```

**TextButton e OutlinedButton**:
```dart
TextButton(
  onPressed: () {},
  child: Text('Texto'),
)

OutlinedButton(
  onPressed: () {},
  child: Text('Contorno'),
)
```

**GestureDetector**: Detecta gestos em qualquer widget
```dart
GestureDetector(
  onTap: () {
    print('Widget tocado');
  },
  child: Container(
    width: 100,
    height: 100,
    color: Colors.blue,
  ),
)
```

---

## StatelessWidget vs StatefulWidget

### StatelessWidget
Um StatelessWidget é **imutável** - uma vez criado, suas propriedades não podem mudar. Use quando o widget não precisa manter estado interno.

```dart
class MeuWidgetSemEstado extends StatelessWidget {
  final String titulo;
  final int numero;
  
  // Construtor - as propriedades são finais
  const MeuWidgetSemEstado({
    Key? key,
    required this.titulo,
    required this.numero,
  }) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text(titulo),
        Text('Número: $numero'),
      ],
    );
  }
}
```

**Quando usar StatelessWidget**:
- Widgets que apenas exibem dados
- Widgets que não mudam ao longo do tempo
- Páginas estáticas
- Widgets de layout puros

### StatefulWidget
Um StatefulWidget pode **mudar** ao longo do tempo. Possui um objeto State associado que mantém informações que podem mudar.

```dart
class Contador extends StatefulWidget {
  @override
  _ContadorState createState() => _ContadorState();
}

class _ContadorState extends State<Contador> {
  int _contador = 0;
  
  void _incrementar() {
    setState(() {
      _contador++;
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Contador: $_contador'),
        ElevatedButton(
          onPressed: _incrementar,
          child: Text('Incrementar'),
        ),
      ],
    );
  }
}
```

**Quando usar StatefulWidget**:
- Formulários
- Animações
- Dados que mudam com interação do usuário
- Timers ou atualizações periódicas

### Ciclo de Vida do StatefulWidget

```dart
class ExemploCicloVida extends StatefulWidget {
  @override
  _ExemploCicloVidaState createState() => _ExemploCicloVidaState();
}

class _ExemploCicloVidaState extends State<ExemploCicloVida> {
  @override
  void initState() {
    super.initState();
    // Chamado UMA vez quando o widget é criado
    print('initState: Widget criado');
  }
  
  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    // Chamado após initState e quando dependências mudam
    print('didChangeDependencies: Dependências mudaram');
  }
  
  @override
  Widget build(BuildContext context) {
    // Chamado sempre que o widget precisa ser reconstruído
    print('build: Reconstruindo widget');
    return Container();
  }
  
  @override
  void didUpdateWidget(ExemploCicloVida oldWidget) {
    super.didUpdateWidget(oldWidget);
    // Chamado quando o widget pai reconstrói este widget
    print('didUpdateWidget: Widget atualizado');
  }
  
  @override
  void dispose() {
    // Chamado quando o widget é removido permanentemente
    print('dispose: Widget sendo destruído');
    super.dispose();
  }
}
```

---

## Layout e Posicionamento

### Widgets de Layout Principais

**Column**: Organiza widgets verticalmente
```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center, // Eixo principal (vertical)
  crossAxisAlignment: CrossAxisAlignment.start, // Eixo cruzado (horizontal)
  children: [
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
)
```

**Row**: Organiza widgets horizontalmente
```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Icon(Icons.home),
    Text('Casa'),
    Icon(Icons.arrow_forward),
  ],
)
```

**Stack**: Sobrepõe widgets
```dart
Stack(
  children: [
    Container(
      width: 100,
      height: 100,
      color: Colors.red,
    ),
    Positioned(
      top: 10,
      left: 10,
      child: Text('Sobreposto'),
    ),
  ],
)
```

**Wrap**: Como Row/Column mas quebra linha quando necessário
```dart
Wrap(
  spacing: 8.0, // Espaçamento horizontal
  runSpacing: 4.0, // Espaçamento vertical
  children: [
    Chip(label: Text('Tag 1')),
    Chip(label: Text('Tag 2')),
    Chip(label: Text('Tag 3')),
  ],
)
```

### Widgets de Espaçamento

**Padding**: Adiciona espaçamento interno
```dart
Padding(
  padding: EdgeInsets.all(16.0),
  child: Text('Com padding'),
)

// Padding específico
Padding(
  padding: EdgeInsets.only(
    top: 8.0,
    bottom: 16.0,
    left: 12.0,
    right: 12.0,
  ),
  child: Text('Padding customizado'),
)
```

**SizedBox**: Cria espaço vazio ou define tamanho
```dart
// Espaçamento vertical
SizedBox(height: 20),

// Espaçamento horizontal
SizedBox(width: 10),

// Tamanho específico
SizedBox(
  width: 200,
  height: 100,
  child: Container(color: Colors.blue),
)
```

**Expanded**: Expande um widget para ocupar espaço disponível
```dart
Row(
  children: [
    Container(width: 50, height: 50, color: Colors.red),
    Expanded(
      child: Container(height: 50, color: Colors.blue),
    ),
    Container(width: 50, height: 50, color: Colors.green),
  ],
)
```

**Flexible**: Similar ao Expanded mas permite que o widget seja menor
```dart
Row(
  children: [
    Flexible(
      flex: 1,
      child: Container(height: 50, color: Colors.red),
    ),
    Flexible(
      flex: 2,
      child: Container(height: 50, color: Colors.blue),
    ),
  ],
)
```

---

## Navegação entre Telas

### Navegação Básica
```dart
// Navegar para nova tela
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => NovatTela()),
);

// Voltar para tela anterior
Navigator.pop(context);

// Substituir tela atual
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => NovatTela()),
);

// Limpar pilha e navegar
Navigator.pushAndRemoveUntil(
  context,
  MaterialPageRoute(builder: (context) => TelaLogin()),
  (route) => false,
);
```

### Passagem de Dados
```dart
// Tela que envia dados
class TelaOrigem extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => TelaDestino(
                  titulo: 'Dados passados',
                  numero: 42,
                ),
              ),
            );
          },
          child: Text('Ir para próxima tela'),
        ),
      ),
    );
  }
}

// Tela que recebe dados
class TelaDestino extends StatelessWidget {
  final String titulo;
  final int numero;
  
  const TelaDestino({
    Key? key,
    required this.titulo,
    required this.numero,
  }) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(titulo)),
      body: Center(
        child: Text('Número recebido: $numero'),
      ),
    );
  }
}
```

### Retornando Dados
```dart
// Tela que aguarda retorno
class TelaEspera extends StatefulWidget {
  @override
  _TelaEsperaState createState() => _TelaEsperaState();
}

class _TelaEsperaState extends State<TelaEspera> {
  String resultado = 'Nenhum resultado';
  
  _navegarEAguardar() async {
    final String? resultado = await Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => TelaRetorno()),
    );
    
    if (resultado != null) {
      setState(() {
        this.resultado = resultado;
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          Text('Resultado: $resultado'),
          ElevatedButton(
            onPressed: _navegarEAguardar,
            child: Text('Obter resultado'),
          ),
        ],
      ),
    );
  }
}

// Tela que retorna dados
class TelaRetorno extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context, 'Dados retornados!');
          },
          child: Text('Retornar dados'),
        ),
      ),
    );
  }
}
```

---

## Gerenciamento de Estado

### setState (Estado Local)
O método mais básico para gerenciar estado em StatefulWidgets:

```dart
class ContadorSimples extends StatefulWidget {
  @override
  _ContadorSimplesState createState() => _ContadorSimplesState();
}

class _ContadorSimplesState extends State<ContadorSimples> {
  int _contador = 0;
  List<String> _itens = [];
  
  void _incrementar() {
    setState(() {
      _contador++;
      _itens.add('Item $_contador');
    });
  }
  
  void _resetar() {
    setState(() {
      _contador = 0;
      _itens.clear();
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Contador: $_contador'),
        Text('Total de itens: ${_itens.length}'),
        Row(
          children: [
            ElevatedButton(
              onPressed: _incrementar,
              child: Text('Incrementar'),
            ),
            ElevatedButton(
              onPressed: _resetar,
              child: Text('Resetar'),
            ),
          ],
        ),
      ],
    );
  }
}
```

### InheritedWidget (Estado Global Simples)
Para compartilhar dados entre widgets sem passar por construtores:

```dart
class DadosApp extends InheritedWidget {
  final String usuario;
  final String tema;
  
  const DadosApp({
    Key? key,
    required this.usuario,
    required this.tema,
    required Widget child,
  }) : super(key: key, child: child);
  
  static DadosApp? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<DadosApp>();
  }
  
  @override
  bool updateShouldNotify(DadosApp oldWidget) {
    return usuario != oldWidget.usuario || tema != oldWidget.tema;
  }
}

// Uso
class WidgetFilho extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final dados = DadosApp.of(context);
    
    return Text('Usuário: ${dados?.usuario ?? 'Desconhecido'}');
  }
}
```

### ChangeNotifier (Estado Reativo)
```dart
class ContadorModel extends ChangeNotifier {
  int _contador = 0;
  
  int get contador => _contador;
  
  void incrementar() {
    _contador++;
    notifyListeners(); // Notifica widgets que escutam
  }
  
  void decrementar() {
    _contador--;
    notifyListeners();
  }
}

// Uso com ChangeNotifierProvider (package: provider)
class AppComProvider extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => ContadorModel(),
      child: MaterialApp(
        home: TelaContador(),
      ),
    );
  }
}

class TelaContador extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Consumer<ContadorModel>(
        builder: (context, contador, child) {
          return Column(
            children: [
              Text('Contador: ${contador.contador}'),
              Row(
                children: [
                  ElevatedButton(
                    onPressed: contador.incrementar,
                    child: Text('+'),
                  ),
                  ElevatedButton(
                    onPressed: contador.decrementar,
                    child: Text('-'),
                  ),
                ],
              ),
            ],
          );
        },
      ),
    );
  }
}
```

---

## Trabalhando com Listas

### ListView Básico
```dart
class ListaSimples extends StatelessWidget {
  final List<String> itens = ['Item 1', 'Item 2', 'Item 3'];
  
  @override
  Widget build(BuildContext context) {
    return ListView(
      children: itens.map((item) => ListTile(
        title: Text(item),
        leading: Icon(Icons.star),
        trailing: Icon(Icons.arrow_forward),
        onTap: () {
          print('$item tocado');
        },
      )).toList(),
    );
  }
}
```

### ListView.builder (Performático)
```dart
class ListaDinamica extends StatelessWidget {
  final List<String> itens = List.generate(100, (i) => 'Item $i');
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: itens.length,
      itemBuilder: (context, index) {
        return Card(
          child: ListTile(
            title: Text(itens[index]),
            subtitle: Text('Subtítulo $index'),
            leading: CircleAvatar(
              child: Text('${index + 1}'),
            ),
            onTap: () {
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('${itens[index]} selecionado')),
              );
            },
          ),
        );
      },
    );
  }
}
```

### Lista com Dados Complexos
```dart
class Produto {
  final String nome;
  final double preco;
  final String imagem;
  
  Produto({required this.nome, required this.preco, required this.imagem});
}

class ListaProdutos extends StatelessWidget {
  final List<Produto> produtos = [
    Produto(nome: 'Produto 1', preco: 29.99, imagem: 'assets/produto1.jpg'),
    Produto(nome: 'Produto 2', preco: 39.99, imagem: 'assets/produto2.jpg'),
  ];
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: produtos.length,
      itemBuilder: (context, index) {
        final produto = produtos[index];
        return ListTile(
          leading: Image.asset(produto.imagem, width: 50, height: 50),
          title: Text(produto.nome),
          subtitle: Text('R\$ ${produto.preco.toStringAsFixed(2)}'),
          trailing: IconButton(
            icon: Icon(Icons.add_shopping_cart),
            onPressed: () {
              // Adicionar ao carrinho
            },
          ),
        );
      },
    );
  }
}
```

### GridView
```dart
class Grade extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GridView.builder(
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2, // 2 colunas
        crossAxisSpacing: 8.0,
        mainAxisSpacing: 8.0,
        childAspectRatio: 1.0, // Proporção quadrada
      ),
      itemCount: 20,
      itemBuilder: (context, index) {
        return Card(
          elevation: 4,
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(Icons.image, size: 50),
              Text('Item $index'),
            ],
          ),
        );
      },
    );
  }
}
```

---

## Formulários e Entrada de Dados

### TextField Básico
```dart
class EntradaTexto extends StatefulWidget {
  @override
  _EntradaTextoState createState() => _EntradaTextoState();
}

class _EntradaTextoState extends State<EntradaTexto> {
  final TextEditingController _controlador = TextEditingController();
  String _texto = '';
  
  @override
  void dispose() {
    _controlador.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        TextField(
          controller: _controlador,
          decoration: InputDecoration(
            labelText: 'Digite algo',
            hintText: 'Placeholder',
            prefixIcon: Icon(Icons.text_fields),
            border: OutlineInputBorder(),
          ),
          onChanged: (valor) {
            setState(() {
              _texto = valor;
            });
          },
        ),
        SizedBox(height: 20),
        Text('Você digitou: $_texto'),
      ],
    );
  }
}
```

### Formulário Completo com Validação
```dart
class FormularioCompleto extends StatefulWidget {
  @override
  _FormularioCompletoState createState() => _FormularioCompletoState();
}

class _FormularioCompletoState extends State<FormularioCompleto> {
  final _formKey = GlobalKey<FormState>();
  final _nomeController = TextEditingController();
  final _emailController = TextEditingController();
  final _senhaController = TextEditingController();
  
  bool _obscurarSenha = true;
  String? _generoSelecionado;
  bool _aceitaTermos = false;
  
  @override
  void dispose() {
    _nomeController.dispose();
    _emailController.dispose();
    _senhaController.dispose();
    super.dispose();
  }
  
  String? _validarEmail(String? valor) {
    if (valor == null || valor.isEmpty) {
      return 'Email é obrigatório';
    }
    if (!valor.contains('@')) {
      return 'Email inválido';
    }
    return null;
  }
  
  String? _validarSenha(String? valor) {
    if (valor == null || valor.isEmpty) {
      return 'Senha é obrigatória';
    }
    if (valor.length < 6) {
      return 'Senha deve ter pelo menos 6 caracteres';
    }
    return null;
  }
  
  void _submeterFormulario() {
    if (_formKey.currentState!.validate() && _aceitaTermos) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Formulário válido!')),
      );
    } else if (!_aceitaTermos) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Você deve aceitar os termos')),
      );
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        children: [
          TextFormField(
            controller: _nomeController,
            decoration: InputDecoration(
              labelText: 'Nome',
              prefixIcon: Icon(Icons.person),
            ),
            validator: (valor) {
              if (valor == null || valor.isEmpty) {
                return 'Nome é obrigatório';
              }
              return null;
            },
          ),
          TextFormField(
            controller: _emailController,
            decoration: InputDecoration(
              labelText: 'Email',
              prefixIcon: Icon(Icons.email),
            ),
            keyboardType: TextInputType.emailAddress,
            validator: _validarEmail,
          ),
          TextFormField(
            controller: _senhaController,
            decoration: InputDecoration(
              labelText: 'Senha',
              prefixIcon: Icon(Icons.lock),
              suffixIcon: IconButton(
                icon: Icon(_obscurarSenha ? Icons.visibility : Icons.visibility_off),
                onPressed: () {
                  setState(() {
                    _obscurarSenha = !_obscurarSenha;
                  });
                },
              ),
            ),
            obscureText: _obscurarSenha,
            validator: _validarSenha,
          ),
          DropdownButtonFormField<String>(
            value: _generoSelecionado,
            decoration: InputDecoration(labelText: 'Gênero'),
            items: ['Masculino', 'Feminino', 'Outro'].map((genero) {
              return DropdownMenuItem(value: genero, child: Text(genero));
            }).toList(),
            onChanged: (valor) {
              setState(() {
                _generoSelecionado = valor;
              });
            },
          ),
          CheckboxListTile(
            title: Text('Aceito os termos e condições'),
            value: _aceitaTermos,
            onChanged: (valor) {
              setState(() {
                _aceitaTermos = valor ?? false;
              });
            },
          ),
          ElevatedButton(
            onPressed: _submeterFormulario,
            child: Text('Cadastrar'),
          ),
        ],
      ),
    );
  }
}
```

---

## Recursos Essenciais

### Diálogos
```dart
// AlertDialog
void _mostrarAlerta(BuildContext context) {
  showDialog(
    context: context,
    builder: (BuildContext context) {
      return AlertDialog(
        title: Text('Confirmação'),
        content: Text('Tem certeza que deseja continuar?'),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: Text('Cancelar'),
          ),
          ElevatedButton(
            onPressed: () {
              Navigator.of(context).pop();
              // Ação confirmada
            },
            child: Text('Confirmar'),
          ),
        ],
      );
    },
  );
}

// BottomSheet
void _mostrarBottomSheet(BuildContext context) {
  showModalBottomSheet(
    context: context,
    builder: (BuildContext context) {
      return Container(
        height: 200,
        child: Column(
          children: [
            ListTile(
              leading: Icon(Icons.photo),
              title: Text('Galeria'),
              onTap: () => Navigator.pop(context),
            ),
            ListTile(
              leading: Icon(Icons.camera),
              title: Text('Câmera'),
              onTap: () => Navigator.pop(context),
            ),
          ],
        ),
      );
    },
  );
}
```

### SnackBar
```dart
void _mostrarSnackBar(BuildContext context, String mensagem) {
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(
      content: Text(mensagem),
      duration: Duration(seconds: 3),
      action: SnackBarAction(
        label: 'Desfazer',
        onPressed: () {
          // Ação de desfazer
        },
      ),
    ),
  );
}
```

### Async/Await e Future
```dart
class ExemploAsync extends StatefulWidget {
  @override
  _ExemploAsyncState createState() => _ExemploAsyncState();
}

class _ExemploAsyncState extends State<ExemploAsync> {
  String _resultado = 'Aguardando...';
  bool _carregando = false;
  
  // Simulação de operação assíncrona
  Future<String> _buscarDados() async {
    await Future.delayed(Duration(seconds: 2)); // Simula delay de rede
    return 'Dados carregados com sucesso!';
  }
  
  void _executarOperacaoAsync() async {
    setState(() {
      _carregando = true;
    });
    
    try {
      String resultado = await _buscarDados();
      setState(() {
        _resultado = resultado;
        _carregando = false;
      });
    } catch (error) {
      setState(() {
        _resultado = 'Erro: $error';
        _carregando = false;
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text(_resultado),
        if (_carregando) CircularProgressIndicator(),
        ElevatedButton(
          onPressed: _carregando ? null : _executarOperacaoAsync,
          child: Text('Buscar Dados'),
        ),
      ],
    );
  }
}
```

### FutureBuilder
```dart
class ExemploFutureBuilder extends StatelessWidget {
  Future<List<String>> _buscarItens() async {
    await Future.delayed(Duration(seconds: 2));
    return ['Item 1', 'Item 2', 'Item 3', 'Item 4'];
  }
  
  @override
  Widget build(BuildContext context) {
    return FutureBuilder<List<String>>(
      future: _buscarItens(),
      builder: (context, snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return Center(child: CircularProgressIndicator());
        } else if (snapshot.hasError) {
          return Center(child: Text('Erro: ${snapshot.error}'));
        } else if (snapshot.hasData) {
          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(snapshot.data![index]),
              );
            },
          );
        } else {
          return Center(child: Text('Nenhum dado encontrado'));
        }
      },
    );
  }
}
```

### StreamBuilder (Dados em tempo real)
```dart
class ExemploStreamBuilder extends StatelessWidget {
  // Stream que emite um número a cada segundo
  Stream<int> _contadorStream() async* {
    int contador = 0;
    while (true) {
      await Future.delayed(Duration(seconds: 1));
      yield contador++;
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return StreamBuilder<int>(
      stream: _contadorStream(),
      builder: (context, snapshot) {
        if (snapshot.hasError) {
          return Text('Erro: ${snapshot.error}');
        } else if (snapshot.hasData) {
          return Text('Contador: ${snapshot.data}');
        } else {
          return Text('Aguardando...');
        }
      },
    );
  }
}
```

### Animações Básicas
```dart
class AnimacaoSimples extends StatefulWidget {
  @override
  _AnimacaoSimplesState createState() => _AnimacaoSimplesState();
}

class _AnimacaoSimplesState extends State<AnimacaoSimples>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;
  
  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );
    _animation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.bounceOut,
    ));
  }
  
  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        AnimatedBuilder(
          animation: _animation,
          builder: (context, child) {
            return Transform.scale(
              scale: _animation.value,
              child: Container(
                width: 100,
                height: 100,
                decoration: BoxDecoration(
                  color: Colors.blue,
                  borderRadius: BorderRadius.circular(50),
                ),
              ),
            );
          },
        ),
        ElevatedButton(
          onPressed: () {
            if (_controller.isCompleted) {
              _controller.reverse();
            } else {
              _controller.forward();
            }
          },
          child: Text('Animar'),
        ),
      ],
    );
  }
}
```

### Temas e Personalização
```dart
class AppComTema extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'App com Tema',
      theme: ThemeData(
        // Cores principais
        primarySwatch: Colors.blue,
        primaryColor: Colors.blue[700],
        accentColor: Colors.orange,
        
        // Tema escuro
        brightness: Brightness.light,
        
        // Tipografia
        textTheme: TextTheme(
          headline1: TextStyle(fontSize: 32, fontWeight: FontWeight.bold),
          bodyText1: TextStyle(fontSize: 16),
        ),
        
        // Botões
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
        
        // AppBar
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.blue[700],
          elevation: 0,
          centerTitle: true,
        ),
      ),
      home: TelaPrincipal(),
    );
  }
}
```

---

## Estrutura de Projeto

### Organização de Pastas
```
lib/
├── main.dart
├── models/
│   ├── usuario.dart
│   ├── produto.dart
│   └── pedido.dart
├── screens/
│   ├── home_screen.dart
│   ├── login_screen.dart
│   └── profile_screen.dart
├── widgets/
│   ├── custom_button.dart
│   ├── product_card.dart
│   └── loading_indicator.dart
├── services/
│   ├── api_service.dart
│   ├── auth_service.dart
│   └── storage_service.dart
├── utils/
│   ├── constants.dart
│   ├── helpers.dart
│   └── validators.dart
└── providers/
    ├── auth_provider.dart
    └── cart_provider.dart
```

### Exemplo de Model
```dart
// models/usuario.dart
class Usuario {
  final int id;
  final String nome;
  final String email;
  final DateTime dataCriacao;
  
  Usuario({
    required this.id,
    required this.nome,
    required this.email,
    required this.dataCriacao,
  });
  
  // Construtor para criar do JSON
  factory Usuario.fromJson(Map<String, dynamic> json) {
    return Usuario(
      id: json['id'],
      nome: json['nome'],
      email: json['email'],
      dataCriacao: DateTime.parse(json['data_criacao']),
    );
  }
  
  // Método para converter para JSON
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'nome': nome,
      'email': email,
      'data_criacao': dataCriacao.toIso8601String(),
    };
  }
  
  // Método copyWith para criar cópias modificadas
  Usuario copyWith({
    int? id,
    String? nome,
    String? email,
    DateTime? dataCriacao,
  }) {
    return Usuario(
      id: id ?? this.id,
      nome: nome ?? this.nome,
      email: email ?? this.email,
      dataCriacao: dataCriacao ?? this.dataCriacao,
    );
  }
}
```

### Exemplo de Service
```dart
// services/api_service.dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class ApiService {
  static const String baseUrl = 'https://api.exemplo.com';
  
  // Método GET
  Future<Map<String, dynamic>> get(String endpoint) async {
    final response = await http.get(
      Uri.parse('$baseUrl$endpoint'),
      headers: {'Content-Type': 'application/json'},
    );
    
    if (response.statusCode == 200) {
      return json.decode(response.body);
    } else {
      throw Exception('Falha ao carregar dados: ${response.statusCode}');
    }
  }
  
  // Método POST
  Future<Map<String, dynamic>> post(String endpoint, Map<String, dynamic> data) async {
    final response = await http.post(
      Uri.parse('$baseUrl$endpoint'),
      headers: {'Content-Type': 'application/json'},
      body: json.encode(data),
    );
    
    if (response.statusCode == 201) {
      return json.decode(response.body);
    } else {
      throw Exception('Falha ao criar recurso: ${response.statusCode}');
    }
  }
}
```

### Widget Customizado Reutilizável
```dart
// widgets/custom_button.dart
class CustomButton extends StatelessWidget {
  final String texto;
  final VoidCallback? onPressed;
  final Color? cor;
  final bool carregando;
  final IconData? icone;
  
  const CustomButton({
    Key? key,
    required this.texto,
    this.onPressed,
    this.cor,
    this.carregando = false,
    this.icone,
  }) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: carregando ? null : onPressed,
      style: ElevatedButton.styleFrom(
        backgroundColor: cor ?? Theme.of(context).primaryColor,
        padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(8),
        ),
      ),
      child: carregando
          ? SizedBox(
              width: 20,
              height: 20,
              child: CircularProgressIndicator(
                strokeWidth: 2,
                valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
              ),
            )
          : Row(
              mainAxisSize: MainAxisSize.min,
              children: [
                if (icone != null) ...[
                  Icon(icone),
                  SizedBox(width: 8),
                ],
                Text(texto),
              ],
            ),
    );
  }
}
```

### Constants e Helpers
```dart
// utils/constants.dart
class AppConstants {
  static const String appName = 'Meu App';
  static const String apiUrl = 'https://api.exemplo.com';
  
  // Cores
  static const Color corPrimaria = Color(0xFF2196F3);
  static const Color corSecundaria = Color(0xFFFF9800);
  
  // Tamanhos
  static const double paddingPadrao = 16.0;
  static const double borderRadius = 8.0;
  
  // Textos
  static const String msgErroConexao = 'Erro de conexão. Tente novamente.';
  static const String msgSucesso = 'Operação realizada com sucesso!';
}

// utils/helpers.dart
class Helpers {
  static String formatarData(DateTime data) {
    return '${data.day.toString().padLeft(2, '0')}/'
           '${data.month.toString().padLeft(2, '0')}/'
           '${data.year}';
  }
  
  static String formatarMoeda(double valor) {
    return 'R\$ ${valor.toStringAsFixed(2).replaceAll('.', ',')}';
  }
  
  static bool isValidEmail(String email) {
    return RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}
            ).hasMatch(email);
  }
}
```

---

## Conceitos Avançados Importantes

### BuildContext
O BuildContext é uma referência à localização de um widget na árvore de widgets. É essencial para:
- Navegar entre telas
- Acessar temas
- Mostrar diálogos e snackbars
- Acessar dados de InheritedWidgets

```dart
// Sempre passe o context correto
void exemploContext(BuildContext context) {
  // Acessar tema
  final tema = Theme.of(context);
  
  // Acessar media query (tamanho da tela)
  final tamanhoTela = MediaQuery.of(context).size;
  
  // Navegar
  Navigator.of(context).push(/* ... */);
  
  // Mostrar snackbar
  ScaffoldMessenger.of(context).showSnackBar(/* ... */);
}
```

### Keys
Keys ajudam o Flutter a identificar widgets únicos:

```dart
// GlobalKey para acessar estado de outros widgets
final GlobalKey<FormState> _formKey = GlobalKey<FormState>();

// ValueKey para listas dinâmicas
ListView.builder(
  itemBuilder: (context, index) {
    return ListTile(
      key: ValueKey(itens[index].id), // Importante para performance
      title: Text(itens[index].nome),
    );
  },
)
```

### Performance
- Use `const` constructors quando possível
- Prefira `ListView.builder` para listas grandes
- Use `RepaintBoundary` para widgets que animam independentemente
- Evite reconstruções desnecessárias com `const` widgets

```dart
// Bom - usa const
const Text('Texto fixo')

// Ruim - recria o widget a cada build
Text('Texto fixo')
```

---

## Próximos Passos

1. **Pratique os conceitos básicos**: Crie pequenos apps testando StatelessWidget, StatefulWidget, navegação e listas.

2. **Aprenda sobre packages**: Explore o pub.dev para packages úteis como:
   - `http` para requisições de rede
   - `shared_preferences` para armazenamento local
   - `provider` para gerenciamento de estado
   - `image_picker` para selecionar imagens

3. **Estude padrões de arquitetura**: MVC, MVP, MVVM, BLoC, Clean Architecture

4. **Explore APIs nativas**: Câmera, GPS, notificações, etc.

5. **Aprenda sobre testes**: Unit tests, widget tests, integration tests

6. **Publique seu primeiro app**: Google Play Store e Apple App Store

---

Este guia cobre todos os fundamentos necessários para começar com Flutter. Pratique cada conceito criando pequenos projetos e gradualmente combine-os em aplicações mais complexas. A chave é praticar consistentemente e construir projetos reais!
