<h3>2023.01</h3> 
<h3>Insight 360</h3>


<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/a26af9a3-6150-462b-83d8-e30eff33e168" />

# Desafio Proposto pelo Cliente 
  
Neste desafio, o cliente foi o [Professor Lucas Gonçalves Nadalete](https://fatecsjc-prd.azurewebsites.net/docentes-bd), que nos apresentou a necessidade de substituir o processo manual de avaliação de desempenho. Esse processo enfrentava diversas limitações, como a dificuldade na consolidação dos dados em planilhas desconectadas, a ausência de padronização nos critérios de avaliação, a lentidão decorrente da burocracia envolvida e a pouca clareza na visualização dos resultados.


# Ferramenta Desenvolvida

Para resolver as limitações do processo manual de avaliação, o grupo [Tech Horizon](https://github.com/TechHorizonBR) desenvolveu o [Insight 360º](https://github.com/Amandavannuccic/API_1_SEMESTRE), um sistema desktop em Python com uma interface amigável e intuitiva construída com Tkinter. A ferramenta permite a visualização gráfica dos resultados por meio do Matplotlib e realiza o armazenamento local dos dados no formato JSON. Além disso, oferece suporte a múltiplos perfis de usuário, distinguindo avaliadores e avaliados, gera relatórios automáticos de desempenho e conta com um sistema básico de autenticação para controle de acesso.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f25f871d-b9b5-45fc-af7c-c38b58b3a621" width="500">


# Tecnologias Utilizadas

| **Categoria**                       | **Ferramenta/Plataforma**     | **Descrição**                                                                 |
|-------------------------------------|-------------------------------|------------------------------------------------------------------------------|
| Linguagem de Programação            | Python                        | Linguagem principal utilizada para o desenvolvimento da aplicação.          |
| IDE (Ambiente de Desenvolvimento)   | VSCode                        | Editor de código utilizado para programar, depurar e organizar o projeto.   |
| Formato de Dados                    | JSON                          | Formato leve e estruturado usado para armazenar os dados localmente.        |
| Front-End                           | Tkinter                       | Biblioteca usada para criar a interface gráfica do usuário (GUI).           |
| Biblioteca de Visualização          | Matplotlib                    | Ferramenta usada para gerar gráficos e visualizações dos resultados.        |
| Plataforma de Produtividade         | Notion                        | Utilizada para organização de tarefas, documentação e anotações do projeto. |
| Plataforma de Comunicação           | Discord                       | Canal principal de comunicação e alinhamento entre os membros do grupo.     |
| Ferramenta de Diagramação           | Diagrams.net                  | Usada para criar diagramas de fluxo, arquitetura e processos do sistema.    |
| Controle de Versão                  | Git                           | Ferramenta para versionamento do código e controle de alterações.           |
| Plataforma de Repositórios          | GitHub                        | Repositório online para colaboração, hospedagem e gerenciamento do código.  |
| Ferramenta de Design Gráfico        | Canva                         | Utilizada para criação de elementos gráficos e visuais para o projeto.      |
| Programa de Planilhas               | Excel                         | Usado para análise e organização de dados em formato tabular.               |
| Método de Gerenciamento de Projetos | Kanban                        | Metodologia ágil usada para acompanhar o progresso e distribuição das tarefas. |

# Contribuições Pessoais

No projeto Insight360º, atuei simultaneamente como Product Owner (PO) e Desenvolvedora, conciliando a mediação entre o cliente (Professor Lucas Nadalete) e a equipe técnica com a implementação direta de funcionalidades essenciais do sistema.

Como PO, fui responsável por compreender as necessidades do cliente, transformar essas demandas em requisitos claros e priorizados, organizar o backlog e assegurar que as entregas estivessem alinhadas aos objetivos definidos. Além disso, contribuí no desenvolvimento do software, criando interfaces gráficas, dashboards analíticos, telas administrativas e documentações técnicas.

### Mediação com o Cliente (PO)
<details>
<summary>Detalhes</summary>

- Tradução das necessidades do professor em requisitos objetivos.  
- Acompanhamento constante das expectativas ao longo das sprints.  
- Priorização do backlog e repasse alinhado das demandas à equipe.  
- Apresentação de entregáveis, protótipos e coleta de feedback.  

</details>

### Definição de Requisitos e Funcionalidades
<details>
<summary>Detalhes</summary>

- Estruturação do backlog e definição das funcionalidades de cada sprint.  
- Detalhamento de regras de negócio para dashboards e tela administrativa.  
- Documentação da Sprint 4 com objetivos, instruções e backlog consolidado.  
- Definição dos indicadores avaliados e fluxos do usuário no sistema.  

</details>

### Desenvolvimento e Implementação
<details>
<summary>Detalhes</summary>

### Dashboard Gerencial – Análise Comparativa

<details> <summary>Código – Processamento e Estrutura do Dashboard</summary>

    def telaDashAnalise():
        global idturma, idtime, sprint
        comp_frame = ctk.CTkFrame(master=janelaDashGerencial, width=800, height=650)
        comp_frame.place(x=200, y=50)
    
    with open("data_json/questions.json", "r") as arquivo:
        dados_json = json.load(arquivo)
    
    # PROCESSAMENTO DE MÉDIAS - TURMA
    resposta1 = resposta2 = resposta3 = resposta4 = resposta5 = 0
    controler = 0
    
    for i in dados_json['avaliacao']:
        if i['idturma'] == idturma:
            for x in i['respostas']:
                controler += 1
                resposta1 += x['resposta1']
                resposta2 += x['resposta2']
                resposta3 += x['resposta3']
                resposta4 += x['resposta4']
                resposta5 += x['resposta5']
    
        # PROCESSAMENTO DE MÉDIAS - TIME
        resposta1Time = resposta2Time = resposta3Time = resposta4Time = resposta5Time = 0
        controlerTime = 0
        
        for i in dados_json['avaliacao']:
            if i['idturma'] == idturma:
                if i['idtime']==idtime:
                    if i['sprint'] == sprintSelecionada:
                        for x in i['respostas']:
                            controlerTime += 1
                            resposta1Time += x['resposta1']
                            resposta2Time += x['resposta2']
                            resposta3Time += x['resposta3']
                            resposta4Time += x['resposta4']
                            resposta5Time += x['resposta5']
</details> 

<details> <summary>Código – Gráfico Comparativo</summary>
  
      # CRIAÇÃO DO GRÁFICO COMPARATIVO
      figura = Figure(figsize=(8,6), dpi=100)
      eixo = figura.add_subplot(111)
      
      eixo.plot(indicadores, valores_time, color="#00FFFF", label="Time", marker='o')
      eixo.plot(indicadores, valores_turma, color="#c8c8c8", label="Turma", marker='s')
      
      eixo.set_title("Análise comparativa entre a turma e o time", color="white")
      eixo.set_facecolor("#404040")
      figura.set_facecolor("#323232")
      
      eixo.scatter(range(len(valores_time)), valores_time, color='#00FFFF')
      eixo.scatter(range(len(valores_turma)), valores_turma, color='#c8c8c8')
      
      for i, valor in enumerate(valores_time):
          eixo.text(i, valor, f'{valor:.2f}', color='#00FFFF', ha='center', va='bottom')
      
      for i, valor in enumerate(valores_turma):
          eixo.text(i, valor, f'{valor:.2f}', color='#c8c8c8', ha='center', va='bottom')
      
      eixo.axhline(y=1, color='gray', linestyle='--')
      eixo.axhline(y=2, color='gray', linestyle='--')
      eixo.axhline(y=3, color='gray', linestyle='--')
      eixo.axhline(y=4, color='gray', linestyle='--')
      eixo.axhline(y=5, color='gray', linestyle='--')
      
      canvas = FigureCanvasTkAgg(figura, master=comp_frame)
      canvas.draw()
      canvas.get_tk_widget().place(x=100, y=80)

</details>

### Tela de Aceite de Usuários

<details> <summary>Código – Exibição de Usuários Não Aceitos</summary>

        frame_2 = ctk.CTkScrollableFrame(master=frame, fg_color='#c0c0c0', width=1000, height=200)
        frame_2.place(x=100, y=40)
        
        acesso = json.load(open("data_json/users.json", "r"))
        ac_turmas = json.load(open("data_json/turmas.json", "r"))
        
        user = acesso["usuarios"]
        tur = ac_turmas["turmas"]
        
        for x in range(len(user)):
            for y in range(len(tur)):
                if(user[x]["idturma"] == tur[y]["idturma"]):
                    turma_certa = tur[y]["nometurma"]
            
            if user[x]["aceito"] == False:
                label_usuario = ctk.CTkLabel(master=frame_2, 
                                            text=f"{user[x]['user']} - {turma_certa}", 
                                            text_color="black", 
                                            font=('Roboto', 12))
                label_usuario.pack(pady=5)

</details> 

<details><summary>Código – Janela de Alerta Personalizada</summary>

        def janela_alert(titulo, mensagem, medida):
            janelaAlerta = ctk.CTk()
            janelaAlerta.title(titulo)
            janelaAlerta.resizable(False, False)
        
            larg_tela = janela.winfo_screenwidth()
            alt_tela = janela.winfo_screenheight()
            x = (larg_tela - medida) // 2
            y = (alt_tela - 100) // 2
        
            janelaAlerta.geometry(f"{medida}x100+{x}+{y}")
        
            ctk.CTkLabel(
                master=janelaAlerta, 
                text=mensagem, 
                font=('Roboto', 15, 'bold')
            ).pack()
        
            def destroy_alerta():
                janelaAlerta.destroy()
        
            ctk.CTkButton(
                janelaAlerta, text="Ok", command=destroy_alerta,
                fg_color='#5CE1E6', text_color='black'
            ).pack()
        
            janelaAlerta.mainloop()

</details>

### Gráfico de Pizza – Atualização

<details> <summary>Código – Pizza Chart</summary>

        figura_pizza = Figure(figsize=(6, 6), dpi=100)
        eixo_pizza = figura_pizza.add_subplot(111)
        
        indicadores = list(dados.keys())
        valores = list(dados.values())
        
        cores = ['#00FFFF', '#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A']
        eixo_pizza.pie(valores, labels=indicadores, colors=cores, autopct='%1.1f%%')
        
        canvas_pizza = FigureCanvasTkAgg(figura_pizza, master=frame_principal)
        canvas_pizza.draw()
        canvas_pizza.get_tk_widget().place(x=500, y=100)

</details>

### Ajustes de Layout e Dimensões

<details> <summary>Código – Centralização</summary>

        screen_width = janelaDashGerencial.winfo_screenwidth()
        screen_height = janelaDashGerencial.winfo_screenheight()
        x = (screen_width - 1200) // 2
        y = (screen_height - 650) // 2
        janelaDashGerencial.geometry("1200x650+{}+{}".format(x, y))

</details>  

</details>


# Hard Skills Desenvolvidas - [Tabela de Níveis](https://github.com/Amandavannuccic/TG-Fatec/blob/main/TabelaDeN%C3%ADveis/TabelaNiveis.md)

### Manipulação de Dados (JSON) — Intermediário
<details> <summary>Detalhes</summary>

- Leitura, escrita e filtragem de dados de usuários, turmas e avaliações.

- Processamento de respostas, médias e agrupamentos por time e sprint.

- Motivo do nível: consigo manipular dados com autonomia, resolver problemas comuns e estruturar fluxos completos.

</details>

### Criação de Interfaces Gráficas (CustomTkinter + Tkinter) — Intermediário
<details> <summary>Detalhes</summary>

- Construção de janelas completas e responsivas.

- Utilização de frames, áreas scrolláveis e navegação lateral.

- Padronização da interface com tema escuro e organização dos elementos.

- Motivo do nível: desenvolvi telas completas sem depender de orientação constante.

</details>

### Visualização de Dados com Matplotlib — Intermediário
<details> <summary>Detalhes</summary>

- Criação de gráficos comparativos com múltiplas séries.

- Implementação de gráfico de pizza, scatter e linhas de referência.

- Integração com Tkinter utilizando FigureCanvasTkAgg.

- Motivo do nível: implementei gráficos complexos, integrei com a GUI e personalizei visualizações com autonomia.

</details>

### Programação em Python 3 — Intermediário
<details> <summary>Detalhes</summary>

- Estruturação de funções, módulos, controle de fluxo e manipulação de arquivos.

- Implementação de cálculos de médias, filtragens e validações.

- Desenvolvimento completo de funcionalidades integradas à GUI.

- Motivo do nível: desenvolvi funcionalidades completas e autônomas, resolvendo problemas recorrentes do projeto.

</details>

### Controle de Versão com Git — Básico
<details> <summary>Detalhes</summary>

- Versionamento das atualizações do projeto.

- Criação de commits descritivos e organizados.

- Manutenção de histórico limpo e estruturado.

- Motivo do nível: utilizo Git com segurança em fluxos simples, mas ainda não aplico estratégias avançadas.

</details>

### Gerenciamento de Repositório com GitHub — Básico
<details> <summary>Detalhes</summary>

- Organização do repositório remoto com versionamento contínuo.

- Publicação de atualizações, documentação e arquivos do projeto.

- Motivo do nível: gerencio repositórios com autonomia, mas ainda sem workflows avançados.

</details>

### Documentação Técnica — Intermediário
<details> <summary>Detalhes</summary>

- Produção do README da Sprint 4 com instruções completas.

- Organização das informações do projeto e detalhamento técnico.

- Motivo do nível: desenvolvi documentação clara, completa e com boa estrutura técnica.

</details>

# Soft Skills Desenvolvidas - [Tabela de Níveis](https://github.com/Amandavannuccic/TG-Fatec/blob/main/TabelaDeN%C3%ADveis/TabelaNiveis.md)

### Trabalho em Equipe — Intermediário
<details> <summary>Detalhes</summary>

- Colaboração contínua entre os membros.

- Comunicação eficiente sobre responsabilidades e progresso.

- Motivo do nível: participei ativamente das discussões e contribuições do time com autonomia.

</details>

### Comunicação Efetiva — Intermediário
<details> <summary>Detalhes</summary>

- Troca de informações via Discord e reuniões periódicas.

- Apresentações regulares do andamento ao cliente.

- Motivo do nível: apresentei resultados, alinhei expectativas e mantive comunicação clara durante todo o projeto.

</details>

### Organização e Planejamento — Intermediário
<details> <summary>Detalhes</summary>

- Uso de backlog e definição clara de prioridades.

- Gerenciamento de prazos durante as sprints.

- Motivo do nível: planejei entregas, defini prioridades e mantive o fluxo do projeto organizado.

</details>

### Autonomia — Intermediário
<details> <summary>Detalhes</summary>

- Desenvolvimento completo de telas e dashboards de forma independente.

- Tomada de decisões técnicas em momentos críticos.

- Motivo do nível: atuei de forma autônoma na maior parte do desenvolvimento.

</details>

### Flexibilidade — Intermediário
<details> <summary>Detalhes</summary>

- Adaptação rápida a mudanças no escopo e novos requisitos.

- Ajustes constantes baseados no feedback do cliente.

- Motivo do nível: adaptei-me facilmente às mudanças e respondi bem às novas demandas.

</details>

### Pensamento Crítico — Intermediário
<details> <summary>Detalhes</summary>

- Avaliação de soluções e refinamento contínuo da usabilidade.

- Identificação de falhas e melhoria de processos.

- Motivo do nível: analisei fluxos, identifiquei problemas e propus soluções com regularidade.

</details>

### Resolução de Problemas — Intermediário
<details> <summary>Detalhes</summary>

- Solução de inconsistências de cálculo e ajustes de layout.

- Tratamento de erros em dados JSON e lógica de exibição.

- Motivo do nível: resolvi desafios técnicos diversos com autonomia.

</details>

### Gestão do Tempo — Intermediário
<details> <summary>Detalhes</summary>

- Organização das tarefas por sprint.

- Cumprimento dos prazos e priorização eficaz.

- Motivo do nível: gerenciei demandas e entreguei conforme os prazos estabelecidos.

</details>
