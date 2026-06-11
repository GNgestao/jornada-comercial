Jornada Comercial — Guia de instalação

App de gestão de visitas do time comercial. Arquivo único (index.html), hospedagem gratuita no GitHub Pages, banco no seu Firebase existente (gn-gestao) usando coleções separadas (jc_usuarios e jc_visitas) — nada do GN Gestão é alterado.


Passo 1 — Criar o repositório no GitHub


Acesse github.com → New repository
Nome: jornada-comercial
Visibilidade: Public (necessário pro GitHub Pages gratuito; o código não tem nenhuma senha, só a config pública do Firebase, que é segura de expor)
Crie o repositório e faça upload do arquivo index.html


Passo 2 — Ativar o GitHub Pages


No repositório → Settings → Pages
Source: Deploy from a branch → Branch: main → pasta / (root) → Save
Em 1-2 minutos a URL fica disponível:
https://SEU-USUARIO.github.io/jornada-comercial/


Passo 3 — Liberar as coleções no Firestore (IMPORTANTE)


Acesse console.firebase.google.com → projeto gn-gestao
Firestore Database → Regras (Rules)
NÃO apague nada do que já existe. Apenas adicione estes dois blocos DENTRO do bloco match /databases/{database}/documents { ... }, junto com as regras que já estão lá:


    match /jc_usuarios/{uid} {
      allow read: if request.auth != null;
      allow create: if request.auth != null && request.auth.uid == uid;
      allow update: if request.auth != null;
    }
    match /jc_visitas/{id} {
      allow read, create, update: if request.auth != null;
    }


Clique em Publicar


Passo 4 — Autorizar o domínio do GitHub Pages


Firebase → Authentication → Settings → Domínios autorizados
Adicione: SEU-USUARIO.github.io
Confirme que o método E-mail/senha está ativado em Authentication → Sign-in method (provavelmente já está, pelo GN Gestão)


Passo 5 — Criar sua conta de superadmin


Abra a URL do app → Cadastre-se
Use o e-mail gabrielnascimento1995@gmail.com — o app reconhece automaticamente como SUPERADMIN
Pronto. Mande a URL pro seu chefe; ele se cadastra e você promove ele a admin na aba Equipe



Como funciona

Vendedor/Consultor:


Digita o nome do prédio + tipo da visita → Iniciar deslocamento (localização e horário de saída registrados automaticamente)
Ao chegar → Cheguei ao local (registra chegada e calcula o tempo de deslocamento)
Ao terminar → Finalizar visita (confirma o tipo, escreve observações; o tempo no local é calculado)
O endereço aproximado é preenchido sozinho (geolocalização) — ele não digita endereço nenhum


Admin (seu chefe): Dashboard com filtros (data, pessoa, tipo, região), ranking de visitas, exportação em PDF, e pode promover membros a admin.

Superadmin (você): Tudo do admin + rebaixar admins, desativar e excluir usuários. Ninguém pode te rebaixar ou excluir — isso é travado no código.

Permissões

AçãoMembroAdminSuperadminCriar/finalizar visitas próprias✅✅✅Ver histórico próprio✅✅✅Dashboard + filtros + PDF❌✅✅Promover membro a admin❌✅✅Rebaixar admin❌❌✅Desativar / excluir usuário❌❌✅

Custos


GitHub Pages: R$ 0
Geolocalização (navegador) + endereço aproximado (OpenStreetMap): R$ 0
Firebase: dentro do plano gratuito pro volume de vocês (6 pessoas, ~dezenas de visitas/dia)
Sem WhatsApp automático nesta versão — quando quiser ativar, aí entra a Evolution API na sua VPS


Limitações desta versão (combinadas)


Relatório chega no dashboard, não no WhatsApp (fase 2)
A localização é capturada nos 3 momentos (saída, chegada, fim) — não há rastreamento contínuo do trajeto
O GPS do celular precisa estar ativo e a permissão de localização concedida ao navegador
