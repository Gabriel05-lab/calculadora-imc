import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// ==========================================
// 1. EXCEÇÃO CUSTOMIZADA (Tratamento de Erros Avançado)
// ==========================================
class DadosInvalidosException extends Exception {
    public DadosInvalidosException(String mensagem) {
        super(mensagem);
    }
}

// ==========================================
// 2. CLASSE ABSTRATA (Abstração Pura)
// ==========================================
abstract class Pessoa {
    private String nome;
    private double peso;
    private double altura;

    public Pessoa(String nome, double peso, double altura) throws DadosInvalidosException {
        if (peso <= 0 || altura <= 0) {
            throw new DadosInvalidosException("Peso e altura devem ser maiores que zero.");
        }
        this.nome = nome;
        this.peso = peso;
        this.altura = altura;
    }

    // Método abstrato: obriga as subclasses a implementarem sua própria lógica
    public abstract double calcularImc();

    // Getters e Setters (Encapsulamento)
    public String getNome() { return nome; }
    public double getPeso() { return peso; }
    public double getAltura() { return altura; }
}

// ==========================================
// 3. HERANÇA E POLIMORFISMO (Classes Filhas)
// ==========================================
class PacienteComum extends Pessoa {
    public PacienteComum(String nome, double peso, double altura) throws DadosInvalidosException {
        super(nome, peso, altura);
    }

    @Override
    public double calcularImc() {
        return getPeso() / (getAltura() * getAltura());
    }
}

class Atleta extends Pessoa {
    // Atributo específico da subclasse
    private double percentualMassaMagra; 

    public Atleta(String nome, double peso, double altura, double percentualMassaMagra) throws DadosInvalidosException {
        super(nome, peso, altura);
        this.percentualMassaMagra = percentualMassaMagra;
    }

    // Polimorfismo: Ajusta o IMC considerando o aviso do enunciado sobre músculos
    @Override
    public double calcularImc() {
        double imcBruto = getPeso() / (getAltura() * getAltura());
        // Ajuste hipotético: atletas com muita massa magra reduzem o impacto do peso bruto no IMC
        if (percentualMassaMagra > 80) {
            return imcBruto * 0.90; 
        }
        return imcBruto;
    }
}

// ==========================================
// 4. CLASSE DE SERVIÇO (Regras de Negócio)
// ==========================================
class CalculadoraIMC {
    public static String obterClassificacao(double imc) {
        if (imc < 18.5) return "Abaixo do peso";
        if (imc <= 24.9) return "Peso normal";
        if (imc <= 29.9) return "Sobrepeso";
        if (imc <= 34.9) return "Obesidade grau I";
        if (imc <= 39.9) return "Obesidade grau II";
        return "Obesidade grau III (mórbida)";
    }
}

// ==========================================
// 5. CLASSE PRINCIPAL (Interface e Fluxo)
// ==========================================
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Pessoa> listaPessoas = new ArrayList<>();
        
        System.out.println("=== SISTEMA AVANÇADO DE TRIAGEM (POO) ===");
        
        while (true) {
            try {
                System.out.print("\nNome (ou 'sair'): ");
                String nome = scanner.nextLine();
                if (nome.equalsIgnoreCase("sair")) break;

                System.out.print("Peso em kg (ex: 75.5): ");
                double peso = Double.parseDouble(scanner.nextLine().replace(',', '.'));

                System.out.print("Altura em metros (ex: 1.75): ");
                double altura = Double.parseDouble(scanner.nextLine().replace(',', '.'));

                System.out.print("É atleta profissional? (S/N): ");
                String ehAtleta = scanner.nextLine();

                Pessoa pessoa;

                if (ehAtleta.equalsIgnoreCase("S")) {
                    System.out.print("Digite o % de massa magra (ex: 85): ");
                    double massa = Double.parseDouble(scanner.nextLine().replace(',', '.'));
                    // Polimorfismo em ação (Instanciando Atleta)
                    pessoa = new Atleta(nome, peso, altura, massa);
                } else {
                    // Instanciando PacienteComum
                    pessoa = new PacienteComum(nome, peso, altura);
                }

                listaPessoas.add(pessoa);
                
                double imc = pessoa.calcularImc();
                System.out.printf("-> IMC Calculado: %.2f (%s)%n", 
                        imc, CalculadoraIMC.obterClassificacao(imc));

            } catch (NumberFormatException e) {
                System.out.println("[ERRO] Digite apenas números válidos.");
            } catch (DadosInvalidosException e) {
                System.out.println("[ERRO] " + e.getMessage());
            }
        }

        // RELATÓRIO FINAL
        System.out.println("\n=======================================================");
        System.out.println("                   RELATÓRIO FINAL                     ");
        System.out.println("=======================================================");
        
        for (Pessoa p : listaPessoas) {
            double imcFinal = p.calcularImc();
            String tipo = (p instanceof Atleta) ? "Atleta" : "Comum";
            
            System.out.printf("Tipo: %-7s | Nome: %-10s | IMC: %.2f | Status: %s%n", 
                    tipo, p.getNome(), imcFinal, CalculadoraIMC.obterClassificacao(imcFinal));
        }
        
        System.out.println("\nSistema finalizado.");
        scanner.close();
    }
}
