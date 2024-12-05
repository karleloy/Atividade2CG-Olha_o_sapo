# Atividade 2 de Computação Gráfica - 2024.3 - Prof Celso Setsuo Kurashima

# Discentes
- Ana Carolina da Silva Martins
- Karl Eloy Henrique
- Francisco Yury Pinheiro Silva

## Vídeo de demonstração

Para ver o jogo funcionando, [clique aqui](https://drive.google.com/file/d/1OGZeqfUua4DHcL3dYxXEdSG1qNi08lK4/view?usp=drive_link).

## Arquivos
Consulte a pasta **Olha_o_sapo** deste repositório para obter os arquivos do projeto.
O programa é baseado nos seguintes arquivos:

**main.cpp**\
Este arquivo contém o ponto de entrada principal do aplicativo de visualização de modelos 3D. Cria uma instância da classe `abcg::Application`, que gerencia o ciclo de vida da aplicação OpenGL. Cria uma instância da classe Window, que define o comportamento da janela e a renderização dos modelos 3D. Define as configurações de OpenGL, como a quantidade de multisampling (samples = 4), que melhora a qualidade visual do modelo e as configurações da janela. Inicia o loop principal da aplicação, que gerencia a renderização, interação com o usuário e eventos da janela. Em caso de erro durante a execução, a aplicação captura exceções do tipo `std::exception` e imprime a mensagem de erro no terminal.

**model.cpp**\
Contém a implementação das principais funcionalidades relacionadas ao carregamento, manipulação e renderização de modelos 3D no aplicativo. Cria os buffers necessários para armazenar os dados do modelo na GPU: Vertex Buffer Object (VBO) e Element Buffer Object (EBO). Carrega um modelo 3D no formato .obj a partir do caminho fornecido com `loadObj(std::string_view path, bool standardize)`. Utiliza a biblioteca `tinyobj::ObjReader` para ler o arquivo. Armazena os vértices únicos e cria uma lista de índices para eliminar duplicação de vértices. Se a opção standardize for verdadeira, centraliza o modelo na origem e normaliza o tamanho para se ajustar ao espaço [-1, 1]. Renderiza o modelo 3D usando OpenGL com `render(int numTriangles) const`; determina o número de triângulos a serem desenhados e executa o comando `glDrawElements` para renderizar os triângulos armazenados no EBO. Configura o Vertex Array Object (VAO), que define como os vértices são processados: inicializa a posição e rotação do modelo, habilita o buffer de profundidade (depth buffering), necessário para a renderização 3D, vincula os atributos dos vértices (posição) ao VAO e configura a matriz de transformação do modelo e as variáveis uniformes relacionadas à projeção, cor e visualização. Desenha o modelo 3D na tela com `paint(glm::ivec2 m_viewportSize, GLuint program)`. Limpa o buffer de cor e o buffer de profundidade. Configura a matriz de transformação do modelo com translação, rotação e escala.

**model.hpp**\
Define a estrutura de dados e as funções para carregar, manipular e renderizar modelos 3D no aplicativo. Ele encapsula o gerenciamento de vértices, índices e buffers OpenGL necessários para a renderização eficiente dos objetos. A estrutura **Vertex** `glm::vec3 position:` define a posição 3D de um vértice no espaço. Carrega um arquivo de modelo 3D no formato .obj com `void loadObj(std::string_view path, bool standardize = true)`. Opcionalmente, o modelo pode ser padronizado para centralizar e normalizar suas dimensões. `void update(float red, float green, float blue)` atualiza os valores RGB para modificar dinamicamente a cor do modelo. Realiza a pintura do modelo no contexto OpenGL, configurando a matriz de modelo e aplicando transformações com `void paint(glm::ivec2 m_viewportSize, GLuint program)`. 

**trackball.cpp**\
 Implementa a classe TrackBall, que gerencia a interação de um trackball virtual para a manipulação de objetos 3D através de movimentos do mouse. Atualiza a rotação do trackball com base na posição atual do mouse com `void mouseMove(glm::ivec2 const &position)`. `project(position):` projeta a posição do mouse em coordenadas normalizadas (NDC) e ajusta para uma esfera virtual. Calcula o eixo de rotação com o produto vetorial entre a posição anterior e a atual. Ajusta a velocidade angular (m_velocity) e a aplica à rotação do objeto e concatena a nova rotação à rotação existente. `void mousePress(glm::ivec2 const &position)` inicia o rastreamento do movimento do mouse. `void mouseRelease(glm::ivec2 const &position)`finaliza o rastreamento do mouse.

**trackball.hpp**\
Gerencia a rotação de objetos 3D através da interação com o mouse. `void mouseMove(glm::ivec2 const &position)` atualiza a rotação do objeto com base na posição atual do mouse. `void mousePress(glm::ivec2 const &position)` inicia o rastreamento do movimento do mouse e registra a posição inicial. `void mouseRelease(glm::ivec2 const &position)` finaliza o rastreamento do mouse e calcula a rotação final. `void resizeViewport(glm::ivec2 const &size)` atualiza o tamanho do viewport para conversão precisa das coordenadas do mouse. `glm::mat4 getRotation() const` retorna a matriz de rotação acumulada do objeto e inclui uma simulação de inércia quando o mouse não está sendo rastreado. `void setAxis(glm::vec3 const axis) noexcept` define o eixo de rotação manualmente. `void setVelocity(float const velocity) noexcept` define a velocidade angular manualmente. Projeta a posição do mouse em uma esfera unitária virtual, simulando a rotação tridimensional `glm::vec3 project(glm::vec2 const &mousePosition) const`.

**window.cpp**\
Gerencia a janela principal da aplicação e a renderização dos modelos 3D. Ele utiliza OpenGL para a renderização gráfica e ImGui para a interface do usuário, permitindo a interação com o objeto exibido através de eventos de mouse e botões de interface. Gerencia os eventos de entrada do usuário, como movimentação do mouse, cliques e rolagem com `void Window::onEvent(SDL_Event const &event)
`. Em `void Window::onCreate()` configura o ambiente OpenGL, carregando shaders e inicializando o modelo 3D definindo a cor de limpeza do buffer de tela como preto, ativa o teste de profundidade para garantir a renderização correta dos objetos 3D, criando o programa OpenGL com os shaders de vértice e fragmento (depth.vert e depth.frag). Além disso carrega o modelo 3D (frog.obj) e configura seu VAO (Vertex Array Object) e armazena o número de triângulos a serem desenhados. `void Window::onUpdate()` atualiza a matriz de modelo e a matriz de visualização a cada frame. `void Window::onPaint()` renderiza o modelo 3D na tela. `void Window::onPaintUI()` desenha a interface gráfica usando ImGui para controle das cores RGB do modelo, permite alterar a projeção entre perspectiva e ortogonal, cria botões para aumentar e diminuir os valores das componentes vermelha, verde e azul do modelo.

**window.hpp**\
Este arquivo contém a declaração da classe `Window`, que herda da classe base `abcg::OpenGLWindow`. A classe Window é responsável por gerenciar os principais eventos e a lógica da aplicação, incluindo:
- Manipulação de eventos do mouse e redimensionamento da janela.
- Configuração e atualização de shaders OpenGL.
- Renderização de um modelo 3D.
- Controle de interface gráfica usando ImGui para alterar as cores do modelo.

A classe Window é o ponto central de controle da aplicação, interagindo com o usuário através de eventos de entrada (mouse) e interface gráfica, e manipulando a renderização 3D do modelo carregado.

**depth.frag**\
Este arquivo define o Fragment Shader da aplicação, responsável por determinar a cor final de cada fragmento (pixel) renderizado na tela. `void main()` é função principal do shader que calcula a cor final do fragmento. `if (gl_FrontFacing)` verifica se o fragmento pertence à face frontal do triângulo renderizado. Se for face frontal, a cor do fragmento é definida diretamente como fragColor. Se for face traseira, a cor do fragmento é alterada para uma tonalidade avermelhada, reduzindo o valor do componente vermelho à metade, enquanto os outros componentes são configurados para zero.

**depth.vert**\
Este arquivo define o Vertex Shader da aplicação, responsável por transformar as coordenadas dos vértices do modelo de um espaço de objeto para o espaço de tela e calcular as propriedades de cor e profundidade dos vértices. Função principal do shader realiza a transformação e o cálculo de cor do vértice:

- Em `vec4 posEyeSpace = viewMatrix * modelMatrix * vec4(inPosition, 1);` a posição do vértice é transformada para o espaço de visão (olho), utilizando a multiplicação da matriz de modelo e a matriz de visualização.
- Em `float i = 1.0 - (-posEyeSpace.z / 3.0);` a profundidade do vértice no espaço de visão é calculada a partir da coordenada Z do vértice (posEyeSpace.z). Esse valor é utilizado para ajustar a intensidade de cor, criando um efeito de profundidade.
- Em `fragColor = vec4(i, i, i, 1) * color;` a cor final do vértice é calculada, ajustando o componente de intensidade i com base na profundidade. Isso cria um efeito visual onde objetos mais distantes se tornam mais escuros.
- Em `gl_Position = projMatrix * posEyeSpace;` a posição do vértice é transformada para o espaço de projeção, preparando o vértice para a renderização na tela.

Este shader aplica um efeito de escurecimento baseado na profundidade. Vértices mais próximos da câmera têm uma cor mais clara, enquanto vértices mais distantes tornam-se progressivamente mais escuros.
