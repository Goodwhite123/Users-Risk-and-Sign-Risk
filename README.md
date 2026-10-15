# 🛡️ Entra ID Identity Protection: High-Risk User Mitigation

Este repositório documenta a implementação de políticas de Conditional Access no Microsoft Entra ID focadas na mitigação automatizada de riscos de identidade, utilizando sinais do **Identity Protection**. O objetivo principal é proteger o ambiente contra contas comprometidas através de respostas automatizadas e exigências rigorosas de autenticação.

1.  **User Risk (Risco de Usuário):** Avalia a probabilidade de a *identidade* ter sido comprometida (ex: credenciais vazadas na internet/dark web). A mitigação ideal envolve forçar o usuário a provar quem é e alterar sua senha de forma segura.

2.  **Sign-in Risk (Risco de Entrada):** Avalia a probabilidade de uma *autenticação específica* não ter sido originada pelo usuário legítimo em tempo real (ex: acesso via navegador anônimo, IP malicioso ou "viagem atípica"). A mitigação envolve bloquear a sessão ou exigir autenticação multifator (MFA) imediata.


## 🎯 Objetivos do Projeto

*   **Mitigação Automatizada:** Responder imediatamente a eventos de "User Risk" elevado, exigindo remediação sem intervenção manual do administrador.
*   **Proteção de Identidade:** Bloquear acesso ou forçar reautenticação forte (MFA) e redefinição de senha segura para contas potencialmente comprometidas.
*   **Validação Segura:** Utilizar o modo `Report-only` para avaliar o impacto das políticas nos logs de auditoria antes da aplicação (enforcement).

## ⚙️ Políticas Implementadas

### 1. CA01-Global-IdentityProtection-UserRisk-High-RequirePasswordReset

Esta política é projetada para remediar o risco de usuários cujas credenciais podem ter sido vazadas ou comprometidas (User Risk).

*   **Assignments:** All users / All cloud apps. (Excluindo contas Break-Glass).
*   **Conditions:**
    *   **User Risk:** High.
*   **Access Controls (Grant):**
    *   `Require risk remediation` (Força a alteração segura da senha).
    *   `Require authentication strength` Nível **Passwordless MFA** (Exige métodos de autenticação multifator robustos). 
*   **Session Controls:**
    *   `Sign-in frequency:` Every time (Garante reautenticação contínua para sessões de alto risco).
*   **State:** `Report-only` (Para fins de auditoria e validação).

  <img width="792" height="858" alt="image" src="https://github.com/user-attachments/assets/352aaabb-8eb6-4bc2-8c9f-0bc1d74e692d" />


### 2. CA02-Global-IdentityProtection-SignInRisk-High-Block

*   **Assignments:** All users / All cloud apps. (Excluindo contas Break-Glass).
*   **Conditions:**
    *   **Sign-in Risk:** High.
*   **Access Controls (Grant):**
    *   `Block access`
*   **Session Controls:**
    *   `Sign-in frequency:` Every time
*   **State:** `Report-only` (Para fins de auditoria e validação).
 
    <img width="662" height="860" alt="image" src="https://github.com/user-attachments/assets/1ec95718-415d-4eb3-8b9a-233f258e9b65" />


## 🛠️ Tecnologias Utilizadas

*   Microsoft Entra ID (Azure AD)
*   Microsoft Entra ID Protection
*   Conditional Access Policies
*   Multifactor Authentication (MFA) Authentication Strengths

## 📈 Impacto e Benefícios

*   Redução drástica do tempo de resposta a incidentes de identidade.
*   Proteção proativa contra acesso não autorizado usando credenciais vazadas.
*   Conformidade com os padrões de segurança modernos focados em identidade como perímetro primário.

---
*Este laboratório prático faz parte do meu desenvolvimento contínuo em segurança de nuvem e arquitetura de identidade corporativa.*
