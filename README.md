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


## Pareamento rápido por código (novo)
1. Atualize/abra o site publicado por HTTPS nos dois Androids.
2. No Android que vai compartilhar a câmera, selecione **Compartilhar minha câmera**, marque a autorização e toque em **Gerar código e compartilhar**. Aceite a permissão de câmera do navegador.
3. Envie o código de 8 caracteres exibido para o outro aparelho, por um canal privado.
4. No aparelho que vai visualizar, selecione **Visualizar câmera de outro aparelho**, digite o código e toque em **Conectar com código**.
5. Para terminar, toque em **Encerrar conexão** no aparelho que compartilha ou no visualizador.

Este pareamento usa PeerJS Cloud como serviço de sinalização e WebRTC para mídia. A disponibilidade depende do serviço externo e das redes usadas. O código não é uma credencial de autenticação forte: trate-o como informação privada e encerre a sessão quando terminar. O protótipo aceita uma chamada ativa por vez, não envia áudio e não substitui autenticação de produção, TURN gerenciado ou pareamento com expiração no servidor.


## Dois PWAs separados
- **Camera (transmissor):** `/camera/` — inicia a câmera após consentimento explícito, mostra um código de pareamento e transmite somente vídeo.
- **Viewer (receptor):** `/viewer/` — recebe o código e mostra a imagem ao vivo.
- Abra ambos por HTTPS. O pareamento usa PeerJS Cloud para sinalização e WebRTC com STUN; algumas redes podem exigir TURN.
- Para encerrar, use o botão de encerramento. Não compartilhe códigos publicamente.

### Limite importante de PWA
Service workers não permanecem em execução contínua e não podem manter a câmera aberta de forma confiável com o app fechado/em segundo plano. Esta versão exige manter a tela Camera aberta. Para transmissão de câmera em segundo plano com notificação permanente no Android, é necessário um aplicativo Android nativo com serviço em primeiro plano, permissões adequadas e controles visíveis. A câmera sempre requer autorização do usuário.


## Redes diferentes (Wi-Fi + dados móveis)
As telas Camera e Viewer agora têm campos opcionais para configurar um servidor TURN:
- Configure o mesmo endpoint TURN nos dois aparelhos.
- Informe usuário e credencial válidos emitidos pelo provedor TURN.
- Mantenha o STUN padrão; o TURN é usado como caminho alternativo quando a conexão direta não funciona.
- Este repositório não inclui um servidor TURN nem credenciais. Sem um TURN válido, a conexão entre redes diferentes pode funcionar em algumas combinações e falhar em outras.
- Não coloque chaves permanentes de servidor em código público. Em produção, gere credenciais TURN temporárias no backend.
- Após atualizar o site, feche/reabra ou recarregue os PWAs para aplicar a nova versão.
