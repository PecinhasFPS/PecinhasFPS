# ImGui Loader Base - Guia de Extensão e Modularização

Este projeto utiliza uma arquitetura modular para o dashboard, facilitando a adição de novas funcionalidades, abas e otimizações. Abaixo, um guia prático para expandir o sistema.

---

## Como adicionar um novo botão à Sidebar (Menu Lateral)

1. **Abra `dashboard/dashboard_types.h`**
   - Edite os arrays `menuItems` e `menuIcons` (definidos como `extern` no header e implementados em `dashboard.cpp`).
   - Exemplo:
     ```cpp
     // dashboard/dashboard_types.h
     extern const char* menuItems[6]; // aumente o tamanho
     extern const char* menuIcons[6];
     ```
     ```cpp
     // dashboard.cpp
     const char* menuItems[6] = { "Menu", "Internet", "Opcionais", "Jogos", "Ajustes", "Nova Aba" };
     const char* menuIcons[6] = { ICON_FA_GRIP_HORIZONTAL, ICON_FA_WIFI, ICON_FA_COG, ICON_FA_GAMEPAD, ICON_FA_ADJUST, ICON_FA_STAR };
     ```
2. **Implemente a renderização da nova aba**
   - Crie um novo módulo, por exemplo, `dashboard_novaaba.cpp/.h`.
   - Declare e implemente a função de renderização, ex: `void DashboardNovaAba::RenderNovaAbaContent();`
   - Inclua o header no `dashboard.cpp` e adicione o case correspondente no switch de `RenderContent()`:
     ```cpp
     #include "dashboard/dashboard_novaaba.h"
     // ...
     switch (selectedMenu) {
         // ...
         case 5: DashboardNovaAba::RenderNovaAbaContent(); break;
     }
     ```

---

## Como adicionar uma nova aba (com conteúdo próprio)

1. **Crie os arquivos do módulo**
   - Exemplo: `dashboard_novaaba.h` e `dashboard_novaaba.cpp` em `dashboard/`.
2. **Declare a função de renderização no header**
   ```cpp
   // dashboard_novaaba.h
   #pragma once
   namespace DashboardNovaAba {
       void RenderNovaAbaContent();
   }
   ```
3. **Implemente a função no .cpp**
   ```cpp
   // dashboard_novaaba.cpp
   #include "dashboard_novaaba.h"
   #include "imgui.h"
   namespace DashboardNovaAba {
       void RenderNovaAbaContent() {
           ImGui::Text("Conteúdo da nova aba!");
       }
   }
   ```
4. **Inclua o header e adicione o case no switch de `RenderContent()`**
   - Veja exemplo acima.

---

## Como adicionar novas funções/otimizações em abas atuais

### Opcionais
1. **Abra `dashboard/cards_opcionais.cpp`**
2. **Adicione um novo `OptionItem` ao vetor**
   ```cpp
   opcionalItems.push_back({
       "NOME DO BOTÃO",
       true, // ou false se não for recomendado
       "\xf0\xXX", // ícone FontAwesome
       "Descrição do que faz",
       { "Benefício 1", "Benefício 2" },
       NomeDaFuncaoDeOtimização // definida em optimizations.cpp
   });
   ```
3. **Implemente a função em `dashboard/optimizations.cpp` e declare em `optimizations.h`**
   ```cpp
   void NomeDaFuncaoDeOtimização() {
       MostrarNotificacao("Minha otimização foi aplicada!");
   }
   ```

### Internet, Ajustes, Jogos
- Siga o mesmo padrão: crie um arquivo `cards_internet.cpp`, `cards_ajustes.cpp`, etc, e modularize o preenchimento dos vetores.
- As funções de ação devem ser implementadas em `optimizations.cpp`.

---

## Como mostrar feedback visual ao usuário
- Use a função `MostrarNotificacao("Mensagem")` em qualquer função de otimização para exibir um toast visual no canto superior direito.

---

## Boas práticas
- Sempre modularize o preenchimento dos vetores de cards para manter o `dashboard.cpp` limpo.
- Centralize as funções de ação em `optimizations.cpp`.
- Use nomes claros e descritivos para novas abas, funções e variáveis.

---

Qualquer dúvida, consulte os exemplos nos arquivos já existentes ou peça sugestões de estrutura aqui! 