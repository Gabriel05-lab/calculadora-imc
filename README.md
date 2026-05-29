calculadora-imc/
│
├── pom.xml
├── README.md
│
└── src/
    └── main/
        └── java/CalculadoraIMC.javadouble calcularIMC(double peso, double altura);

String classificarIMC(double imc);

}PessoaBase.java
protected String nome;
protected int idade;
public abstract String exibirPerfil();
getNome()
getIdade()
Pessoa.java
extends PessoaBase
implements CalculadoraIMC
private double peso;
private double altura;
private boolean ativo;
nome
idade
peso
altura
super(nome, idade);
getPeso()
getAltura()
isAtivo()
setPeso()
peso > 0
peso / (altura²)
if
else if
else
<18.5
18.5–24.9
25–29.9
30–34.9
35–39.9
>=40
Pessoa: João | Idade: 25
Atleta.java
extends Pessoa
private String modalidade;
nome
idade
peso
altura
modalidade
super(...)
@Override
public String classificarIMC(...)
<20
20–27
>27
super.exibirPerfil()
CalculadoraRecursiva.java
potencia(double base, int exp)
exp == 0
1
base * potencia(base, exp - 1)
arredondar()
mostrarSeparador()
EntradaInvalidaException.java
RuntimeException
public EntradaInvalidaException(String mensagem)
super(mensagem);
Historico.java
ArrayList<String>
adicionar()
exibir()
Nenhum cálculo registrado.
for(String r : registros)
SistemaIMC.java
private Historico historico =
        new Historico();
        processar(Pessoa pessoa)
        exibirHistorico()
        historico.exibir();
        Main.java
        Scanner scanner
        SistemaIMC sistema =
        new SistemaIMC();
        Pessoa pessoaAtual = null;
        Pessoa
        Atleta
        lerInt()
lerDouble()
lerString()
try/catch
EntradaInvalidaException
while(opcao != 0)
1 - Cadastrar Pessoa
2 - Cadastrar Atleta
3 - Calcular IMC
4 - Histórico
0 - Sair
switch(opcao)
new Pessoa(...)
new Atleta(...)
if(pessoaAtual != null)
sistema.processar(pessoaAtual);
EntradaInvalidaException
calcularIMC()
altura * altura
CalculadoraRecursiva.potencia(altura, 2)
<groupId>br.edu.seunome</groupId>
<artifactId>calculadora-imc</artifactId>
<version>1.0.0</version>
17
JUnit Jupiter# calculadora-imc
