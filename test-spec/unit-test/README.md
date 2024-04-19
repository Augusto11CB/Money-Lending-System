# Teste de Unidade Servico Simulação

## Tools para Teste
- JUnit 5
- Mockito

## 1. Exemplo
- Teste para verificar se a simulação é corretamente gerada com os dados fornecidos:

```
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.Test;

class SimulacaoServiceTest {

    @Test
    void testSimulateLoan() {
        // Configuração dos mocks
        ScoreService scoreServiceMock = mock(ScoreService.class);
        when(scoreServiceMock.getScore(anyString())).thenReturn(800);

        TaxService taxServiceMock = mock(TaxService.class);
        when(taxServiceMock.getInterestRate(anyInt())).thenReturn(0.05);

        // Configuração do objeto sob teste
        SimulacaoService simulacaoService = new SimulacaoService(scoreServiceMock, taxServiceMock);

        // Execução do método sob teste
        SimulacaoResult result = simulacaoService.simulateLoan(10000, 12, 3000);

        // Verificação dos resultados esperados
        assertEquals(15000, result.getTotalValue());
        assertEquals(1000, result.getInstallmentValue());
    }
}

```

## 2. Exemplo
- Teste para verificar se a simulação retorna corretamente uma exceção quando o score é inferior ao mínimo necessário:

```
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.Test;

class SimulacaoServiceTest {

    @Test
    void testSimulateLoanWithLowScore() {
        // Configuração dos mocks
        ScoreService scoreServiceMock = mock(ScoreService.class);
        when(scoreServiceMock.getScore(anyString())).thenReturn(500);

        TaxService taxServiceMock = mock(TaxService.class);

        // Configuração do objeto sob teste
        SimulacaoService simulacaoService = new SimulacaoService(scoreServiceMock, taxServiceMock);

        // Execução do método sob teste e verificação da exceção
        assertThrows(ScoreInsufficientException.class, () -> {
            simulacaoService.simulateLoan(10000, 12, 3000);
        });
    }
}
```

## 3. Exemplo

- Teste para verificar se a simulação retorna corretamente uma exceção quando ocorre um erro ao buscar as taxas de juros:

```
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.Test;

class SimulacaoServiceTest {

    @Test
    void testSimulateLoanWithTaxError() {
        // Configuração dos mocks
        ScoreService scoreServiceMock = mock(ScoreService.class);
        when(scoreServiceMock.getScore(anyString())).thenReturn(800);

        TaxService taxServiceMock = mock(TaxService.class);
        when(taxServiceMock.getInterestRate(anyInt())).thenThrow(new TaxServiceException("Erro ao buscar taxas"));

        // Configuração do objeto sob teste
        SimulacaoService simulacaoService = new SimulacaoService(scoreServiceMock, taxServiceMock);

        // Execução do método sob teste e verificação da exceção
        assertThrows(TaxServiceException.class, () -> {
            simulacaoService.simulateLoan(10000, 12, 3000);
        });
    }
}
```