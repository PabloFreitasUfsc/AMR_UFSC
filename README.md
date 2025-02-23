# Implementação de Veículo de Carga Autônomo

**Ainda em contrução**

## Introdução

Objetivo de construir um robô protótipo para cargas em locais fechados como armazens. Esse arquivo tem a função de guiar como utilizar os códigos presentes nesse site.


## Pre-requisitos

### Precisa ter instalado no pc:

*Ubuntu 18.04*, *ssh* ativado, *ROS melodic*, *ROS workspace* ( https://catkin-tools.readthedocs.io/en/latest/# ), alguns pacotes do *ROS* **(TODO listar os pacotes necessários para usar no apt-get install)**.

#### *Raspberry* com *Ubuntu Mate 18.04* (https://ubuntu-mate.org/download/)

 *ROS workspace* ( usado o *catkin_make* por ser um ambiente com processamento limitado), vesão limita do *ROS melodic 18.04*, não é preciso instalar tudo, vou listar todos os pacotes necessarios.

Ter o *arduino-ide* (https://www.arduino.cc/en/main/software) e energia (https://energia.nu/) baixados.


# Instalação códigos fonte no PC (monitorador)


Primeiramento no pc(monitorador), entre na área de trabalho do *ROS (github_ws)*:


```bash
cd github_ws/src/
```

Baixe a ultima versão do código fonte:


```bash
git clone https://github.com/PabloFreitasUfsc/AMR_UFSC
```

Volte um diretório e limpe a área de trablho:

```bash
cd .. && catkin clean -y
```
Re-construa o espaço:

```bash
catkin build
```
**Feche o terminal e abra um novo toda vez que recompilar os códigos.**






# Instalação códigos fonte no Raspberry Pi (RPI)

Por fim no *RPI*, entre na area de trabalho do *ROS (github_ws)*:

```bash
cd github_ws/src/
```
Baixe a ultima versão do código fonte:

```bash
git clone https://github.com/PabloFreitasUfsc/AMR_UFSC
```
Volte um diretório e limpe a área de trabalho:


```bash
cd .. && catkin clean -y
```
Re-construa o espaço:

```bash
catkin_make -j2
```

Obs: Foi utilizado o comando -j2 para usar dois núcleos do raspberry pi, ele possui alguns problemas para compilar os códigos recomendo da uma lida sobre o swap do sistema operacional.

**Feche o terminal e abra um novo toda vez que recompilar os códigos.**


# Verificando o funcionamento do microcontrolador

Após carregar o código no *microcontrolador(uC)*, podendo ser o **Arduino nano** ou **Tiva C**, possuindo pastas especificas para cada um ( **~/github_ws/src/AMR_UFSC/closed_loop**). Abra um terminal e execute o comando abaixo para visualizar a resposta serial:

**Obs: Se somente tiver um *uC* conectado no computador, utilizar o completa automático ( usar o atalho do tab) a partir do ...ACM para descobrir qual porta o uC se encontra.**

```bash
miniterm /dev/ttyACM0 115200
```
**TODO, colocar uma IMAGEM da saída**

Na documentação explicarei mais detalhadamente.

Logo em seguida é possivel testar os comandos de entrada:


Reset do microcontrolador:

*r<espaço><enter>*

Mandar mensagem para os motores:

*s<espaço><velocidade_esqueda><espaço><velocidade_direita><espaço>*



# Controle pelo teclado


Inicializa o microcontrolador e os sensores*(imu,encoders,ponte-h)* no *ROS*:


```bash
roslauch pegasus_bringup inicial.launch
```

Comunicação entre o teclado e as mensagens enviadar para o motor:

```bash
roslaunch pegasus_bringup keyboard_teleop.launch
```

# Mapeamento


Inicializa o microcontrolador e os sensores*(imu,encoders,ponte-h)* no *ROS*:

**Obs:Como estamos trabalhando com mapeamento, deve-se verificar o parâmetro *cmd_vel_topic*, deve se possuir essa linha de comendo no arquivo *~/launch/inicial.launch*:**

```launch
...
<rosparam param="cmd_vel_topic">"cmd_vel_mux/input/teleop"</rosparam>
...
```
Se o parâmetro estiver correto prosiga com a execução em um terminal:
```bash
roslauch pegasus_bringup inicial.launch
```

Ativando o lidar

```bash
roslauch pegasus_bringup lidar.launch
```

Carregando o algoritmo SLAM ( Simultaneas Localization and Mapping), nesse caso sera usado o gmapping:
```bash
roslaunch pegasus_bringup mapeamento_demo.launch

```
Abrir um arquivo Rviz para vizualiação da criação do mapa:

```bash
roslaunch pegasus_bringup mobot_rviz_navegacao.launch
```

Por fim para salvar o mapa feito, onde o argumento ~/github_ws/src/AMR_UFSC/pegasus_bringup/map/test_map2 pode ser trocado pelo diretório e nome do arquivo que queira salvar:


```bash
rosrun map_server map_saver -f ~/github_ws/src/AMR_UFSC/pegasus_bringup/map/test_map2
```

# Navegação


Inicializa o microcontrolador e os sensores*(imu,encoders,ponte-h)* no *ROS*:

```bash
roslauch pegasus_bringup inicial.launch
```

Ativando o *lidar*:

```bash
roslauch pegasus_bringup lidar.launch
```

Carrega o mapa que o robô precisará navegar:

```bash
roslaunch pegasus_bringup amcl_demo.launch

```
Abrir um arquivo *Rviz* para vizualiação da criação do mapa:

```bash
roslaunch pegasus_bringup mobot_rviz_navegacao.launch
```

## Notas de Desenvolvimento

Link para a documentação:
<https://drive.google.com/open?id=1Uiy5zNxnDLPrxDTVcyMnjF-LcmeB3GVd0uSXu1x8qcg>



## Author

-   **Adriano**
-   **André**
-   **Jean**
-   **Lígia**
-   **Pablo Freitas Santos** - _Acesso ao Git_ - [PabloFreitasUfsc](https://github.com/PabloFreitasUfsc)


## Acknowledgments

-   <https://github.com/PurpleBooth>
- <https://catkin-tools.readthedocs.io/en/latest/#>
