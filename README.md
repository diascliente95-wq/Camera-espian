# Camera Espian — câmera remota autorizada

## O que esta versão faz
- Pede autorização explícita antes de abrir a câmera no aparelho que compartilha.
- Usa WebRTC para transmitir apenas vídeo (sem áudio).
- Permite troca manual de oferta/resposta entre os dois aparelhos; não existe servidor de sinalização neste protótipo.
- Exibe o estado da sessão e permite encerrar, desligando as faixas da câmera.

## Como testar
1. Publique o repositório com GitHub Pages ou outro host HTTPS. A API de câmera exige contexto seguro.
2. Abra o mesmo endereço nos dois aparelhos Android, preferencialmente com o Chrome atualizado.
3. No aparelho que vai compartilhar: selecione **Android que compartilha a câmera**, marque a autorização, toque em **Iniciar compartilhamento**, permita a câmera e toque em **Criar convite**.
4. Copie o código gerado e envie-o diretamente para o aparelho visualizador.
5. No visualizador: selecione **Aparelho que vai visualizar**, cole o convite em **Cole aqui o código recebido** e toque em **Usar código recebido**. A resposta será criada.
6. Envie a resposta de volta ao aparelho que compartilha; cole-a no campo de código recebido e toque em **Aplicar resposta**.
7. Para terminar, toque em **Encerrar sessão** em cada aparelho.

## Limites importantes
- Os códigos de sinalização contêm dados temporários da conexão. Não os publique nem os envie a pessoas não autorizadas.
- A conexão usa um servidor STUN público para tentar atravessar NAT. Algumas redes móveis, roteadores ou firewalls exigem um serviço TURN. O protótipo não configura TURN nem garante conexão em todas as redes.
- Para um produto de uso contínuo, substitua a troca manual por sinalização autenticada via HTTPS/WebSocket, códigos de pareamento de uso único, expiração curta, limites de tentativas, auditoria e um TURN gerenciado com credenciais temporárias.
- A câmera só é solicitada após a ação visível da pessoa e a confirmação no aparelho que compartilha. Não use o sistema para vigiar alguém sem consentimento.
