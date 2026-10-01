# Presença — controlo de funcionários

Aplicação responsiva em HTML, CSS, JavaScript e Bootstrap 5, com Supabase como backend.

## Níveis de acesso
- **Administrador:** gere funcionários e contas, define níveis de acesso, regista presenças e exporta relatórios gerais.
- **Supervisor:** regista entradas/saídas e consulta relatórios da equipa; não administra contas nem cadastros de funcionários.
- **Funcionário:** regista entrada/saída e consulta/exporta apenas os próprios registos.

As permissões também são aplicadas no Supabase com Row Level Security (RLS), não apenas escondendo botões no ecrã.

## Configuração inicial
1. Cria um projeto no Supabase.
2. Abre **SQL Editor**, cola o conteúdo de `supabase-schema.sql` e executa-o.
3. Em **Project Settings > API**, copia o **Project URL** e a chave **anon/public**.
4. Abre `app.js` e substitui `COLOCA_AQUI_A_URL_DO_PROJETO` e `COLOCA_AQUI_A_CHAVE_ANON_PUBLIC`.
5. Publica a pasta num alojamento estático ou testa num servidor local.
6. No app, cria a primeira conta. Se for exigida confirmação de e-mail, confirma-a.
7. No SQL Editor, promove essa conta inicial a administradora. Troca o e-mail de exemplo pelo e-mail usado no registo:

```sql
update public.profiles
set role = 'admin', employee_id = null
where lower(email) = lower('admin@tuaempresa.com');
```

8. Inicia sessão novamente. A área **Contas e níveis de acesso** permite atribuir Administrador, Supervisor ou Funcionário às contas registadas. Para um perfil Funcionário, escolhe também o funcionário correspondente.
9. Para novos utilizadores: primeiro cadastra o funcionário, depois a pessoa cria uma conta e o administrador associa a conta ao cadastro na área de acessos.

## Teste local
Dentro desta pasta, executa `python -m http.server 8000` e abre `http://localhost:8000`. Configura também essa origem em **Authentication > URL Configuration** no Supabase.

## Segurança
- No frontend usa-se apenas a chave **anon/public**. Nunca coloques a chave **service_role** no navegador.
- As contas novas começam como Funcionário sem associação; não conseguem ver presenças até o administrador as associar.
- O primeiro administrador é promovido manualmente no SQL Editor. A aplicação impede remover o último administrador.
- Relatórios são descarregados em CSV, compatível com Excel; os horários são apresentados no fuso de Angola (UTC+1).
- Antes de servir várias empresas/clientes, acrescenta isolamento por organização (tenant) e políticas próprias para cada empresa.
