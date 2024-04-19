# Teste de Integração

## Tools para Teste
- Cucumber
- Java
- Rest Assured


## 1. Exemplo

- O teste abrange chamadas para APIs de simulação, formalização e crédito, validando o fluxo completo.


```
Feature: Simulação, Contratação, Formalização e Solicitação de Crédito
  
  Como um cliente,
  Quero realizar o processo completo de simulação, contratação, formalização e solicitação de crédito,
  Para obter um empréstimo.
  
  Cenário: Simular, contratar, formalizar e solicitar crédito
    Dado que tenho acesso às APIs de Simulação, Formalização e Crédito
    Quando informo meus dados na API de Simulação e recebo as propostas
    E contrato uma das propostas retornadas
    E assino o contrato retornado pela API de Formalização
    Então a API de Crédito é acionada com os dados do contrato para solicitar o crédito
    E recebo a resposta da API de Crédito com o status da solicitação


```


```
import io.cucumber.java.en.*;
import org.mockito.Mockito;

import static org.mockito.Mockito.*;

public class SimulacaoContrataFormalizaSolicitaCreditoSteps {

    private SimulacaoAPI simulacaoAPI;
    private ContratoAPI contratoAPI;
    private CreditoAPI creditoAPI;

    private Proposta simulacaoProposta;
    private Contrato contrato;

    @Given("que tenho acesso às APIs de Simulação, Formalização e Crédito")
    public void inicializarAPIs() {
        simulacaoAPI = mock(SimulacaoAPI.class);
        contratoAPI = mock(ContratoAPI.class);
        creditoAPI = mock(CreditoAPI.class);
    }

    @When("informo meus dados na API de Simulação e recebo as propostas")
    public void simular() {
        simulacaoProposta = simulacaoAPI.simular(criarDadosSimulacao());
    }

    @And("contrato uma das propostas retornadas")
    public void contratarProposta() {
        contrato = contratoAPI.contratar(simulacaoProposta.getId());
    }

    @And("assino o contrato retornado pela API de Formalização")
    public void assinarContrato() {
        contratoAPI.assinar(contrato.getId());
    }

    @Then("a API de Crédito é acionada com os dados do contrato para solicitar o crédito")
    public void solicitarCredito() {
        creditoAPI.solicitar(contrato);
    }

    @And("recebo a resposta da API de Crédito com o status da solicitação")
    public void verificarRespostaCredito() {
        String status = creditoAPI.getSituacaoSolicitacao(contrato.getId());
        // Validar status da solicitação (aprovado, recusado, em análise, etc.)
    }

    private DadosSimulacao criarDadosSimulacao() {
        // Implementar a criação dos dados de simulação
        return new DadosSimulacao();
    }
}

```