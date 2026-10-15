# 🛡️ Entra ID Identity Protection: Mitigação de Risco com Conditional Access

## 🧠 Conceitos Aplicados: User Risk vs. Sign-in Risk

Uma arquitetura de segurança robusta precisa tratar diferentes tipos de ameaças de forma distinta. Este laboratório implementa políticas separadas para os dois tipos de risco avaliados pelo Entra ID:

1.  **User Risk (Risco de Usuário):** Avalia a probabilidade de a *identidade* ter sido comprometida (ex: credenciais vazadas na internet/dark web). A mitigação ideal envolve forçar o usuário a provar quem é e alterar sua senha de forma segura.

3.  **Sign-in Risk (Risco de Entrada):** Avalia a probabilidade de uma *autenticação específica* não ter sido originada pelo usuário legítimo em tempo real (ex: acesso via navegador anônimo, IP malicioso ou "viagem impossivel"). A mitigação envolve bloquear a sessão ou exigir autenticação multifator (MFA) imediata.

## ⚙️ Arquitetura das Políticas

Ambas as políticas foram implementadas inicialmente em estado de **Report-only** para auditoria de logs e validação de impacto antes do *enforcement* em produção.

### 1. Require trade password for risky users (Foco em User Risk)
Esta política atua sobre contas potencialmente vazadas, forçando a remediação do risco sem intervenção do Helpdesk.

*   **Assignments:** All users / All cloud apps.
*   **Conditions -> User Risk:** High.
*   **Access Controls (Grant):** 
    *   `Require risk remediation` (Exige redefinição de senha segura).
    *   `Require authentication strength` (Exige métodos de MFA fortes, barrando *credential stuffing*).
*   **Session Controls:** `Sign-in frequency: Every time` (Garante que sessões não sejam reaproveitadas).

  <img width="660" height="752" alt="image" src="https://github.com/user-attachments/assets/b287c3f5-c23d-40e6-b54d-2609ab4be6bb" />


### 2. Bloquear acesso para Sign-in-Risk High (Foco em Sign-in Risk)
Esta política atua em tempo real contra tentativas de login anômalas ou vindas de fontes maliciosas conhecidas.

*   **Assignments:** All users / All cloud apps.
*   **Conditions -> Sign-in Risk:** High.
*   **Access Controls (Grant):** `Block access` (Bloqueia preventivamente a tentativa de entrada até que o risco seja investigado ou caia para um nível aceitável).

## 🛠️ Tecnologias Utilizadas

*   Microsoft Entra ID (Azure AD)
*   Microsoft Entra ID Protection (P2)
*   Conditional Access Policies
*   MFA Authentication Strengths

## 📈 Impacto

*   Separação lógica na resposta a incidentes (vazamento de credencial vs. anomalia de sessão).
*   Redução drástica da superfície de ataque e do tempo de resposta a identidades comprometidas.
*   Automação da segurança com zero atrito para usuários fora da zona de risco.
