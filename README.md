# 07
CLASSE LEITOR (metodo para ler e carregar os capitulo e caracteristica dos personagem)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
_import java.io.File;
import java.io.FileNotFoundException;
import java.util.HashMap;
import java.util.Scanner;

public class Leitor{


  HashMap<String,Caracteristicapersonagem> ldPersonagens(String direcaoarquivo){

   HashMap<String,Caracteristicapersonagem> personagens=new HashMap<String,Caracteristicapersonagem>();
    File  ddPersonagens=new File( direcaoarquivo);
    
    try {
    Scanner escaneadordirecaoarquivo = new Scanner(ddPersonagens,"UTF-8"); 



       String nomeCaracteristica="";
       String le="";
       int energiaCaracteristica=0;

    while(escaneadordirecaoarquivo.hasNextLine())
     {
       while(!le.equals("PERSONAGEMHT")){
         
       
             le=escaneadordirecaoarquivo.nextLine();
         
        }
        le=escaneadordirecaoarquivo.nextLine();
        nomeCaracteristica=escaneadordirecaoarquivo.nextLine();
        le=escaneadordirecaoarquivo.nextLine();
        energiaCaracteristica= Integer.parseInt(escaneadordirecaoarquivo.nextLine());       
        personagens.put(nomeCaracteristica,new Caracteristicapersonagem(nomeCaracteristica,energiaCaracteristica));
      
    } 
         
      
       escaneadordirecaoarquivo.close();




    } catch (FileNotFoundException exception) {
      
      exception.printStackTrace();
    }



 return personagens;
}

  HashMap<String,Capitulo> ldcapitulos(String direcaoarquivocapitulo, HashMap<String,Caracteristicapersonagem> personagens ,Scanner escanear)
  {
   HashMap<String,Capitulo> capitulos=new HashMap<String,Capitulo>();
   File  ddcapitulos=new File( direcaoarquivocapitulo);
   
   try {
   Scanner escaneadordirecaocapitulos= new Scanner(ddcapitulos,"UTF-8"); 



     
  String le="";
      

   while(escaneadordirecaocapitulos.hasNextLine())
    {
      while(! le.equals("CAPITULOS") && 
            ! le.equals("ESCOLHAS"))
      {
            le=escaneadordirecaocapitulos.nextLine();  
          }
                       
            if(le.equals("CAPITULOS")){
               
               ldcapitulos( personagens, 
                            escanear,
                            capitulos,
                            escaneadordirecaocapitulos); 
                            le="";

            }
            else if(le.equals("ESCOLHAS")){
             ldescolhas(capitulos,escaneadordirecaocapitulos);
             le="";

            }           
       
   
   } 
        
     
      escaneadordirecaocapitulos.close();




   } catch (FileNotFoundException exception) {
     
     exception.printStackTrace();
   }



return capitulos;

  }

  private String  ldcapitulos(HashMap<String, Caracteristicapersonagem> personagens, Scanner escanear,
      HashMap<String, Capitulo> capitulos, Scanner escaneadordirecaocapitulos) {
    String nomeCapitulo;
    String textoCapitulo;
    String nomepersonagem;
    int variacaoenergia;
    String le;
    le=escaneadordirecaocapitulos.nextLine();
     nomeCapitulo=escaneadordirecaocapitulos.nextLine();
     le=escaneadordirecaocapitulos.nextLine();
     textoCapitulo=escaneadordirecaocapitulos.nextLine();
     le=escaneadordirecaocapitulos.nextLine();
     nomepersonagem=escaneadordirecaocapitulos.nextLine();                                                                
     le=escaneadordirecaocapitulos.nextLine();
     variacaoenergia=Integer.parseInt(escaneadordirecaocapitulos.nextLine());       
     capitulos.put(nomeCapitulo,new Capitulo(nomeCapitulo,
                                             textoCapitulo,
                                             personagens.get( nomepersonagem) ,
                                             variacaoenergia,
                                             escanear));
    return le;
  }

private String ldescolhas(HashMap<String, Capitulo> capitulos, Scanner escaneadordirecaocapitulos) {
    String nomeCapitulosaida;
    String textoescolha;
    String nomeCapitulochegada;
    String le;
    le=escaneadordirecaocapitulos.nextLine();
     nomeCapitulosaida=escaneadordirecaocapitulos.nextLine();
     le=escaneadordirecaocapitulos.nextLine();
     textoescolha=escaneadordirecaocapitulos.nextLine();
     le=escaneadordirecaocapitulos.nextLine();
     nomeCapitulochegada=escaneadordirecaocapitulos.nextLine();
     capitulos.get(nomeCapitulosaida).escolhas.add( new Escolha(textoescolha,capitulos.get(nomeCapitulochegada)));                                     
     
     return le;
           }

  
}____________________________________________
PROGRAMA PRINCIPAL

-----------------------------------------
import java.util.HashMap;
import java.util.Scanner;


/**
 * App
 */
public class App {

    /**
    * @param args
    */
   public static void main(String[] args) throws Exception{
    Scanner his=new Scanner(System.in);

    Leitor leitor=new Leitor();
    HashMap<String,Caracteristicapersonagem>personagens=leitor.ldPersonagens("pj/armazena dados");
    HashMap<String,Capitulo> capitulos=leitor.ldcapitulos("pj/capitulos",personagens,his);
    Capitulo raiz=capitulos.get("A  TRILHA PELA FLORESTA");
            
    raiz.mostra(); 
    his.close();
          
   }
    
   }
______________________________________________________________________________________________________________________________________________
_______/////________///////_______/////______/////_____///////_________///////____//////////_____________///////_______________________________
   
ARQUIVO ARMAZENA DADOS
----------------------------
PERSONAGEMHT
NOME:
martin
ENERGIA:
100

PERSONAGEMHT
NOME:
assassino
ENERGIA:
100

PERSONAGEMHT
NOME:
jojoana
ENERGIA:
100

PERSONAGEMHT
NOME:
joao
ENERGIA:
300
_____________________________________________________________________________________________________________________________________________________________
____________///////_______////////__________///////___________________/////////______________________////////__________________//////________________________

ARQUIVO CAPITULO
------------------------------------
CAPITULOS
NOME:
A  TRILHA PELA FLORESTA
TEXTO:
Um certo dia três amigos,jasom,martin e joana,resoveram fazer uma trilha pela floresta,chegando lá  eles avistaram uma  casa abandonada.O que eles tem que fazer?
PERSONAGEM:
joana
VARIAÇÃO ENERGIA:
0

CAPITULOS
NOME:
DIA NORMAL
TEXTO:
E assim foi mais um dia normal na vida deles!
PERSONAGEM:
joana
VARIAÇÃO ENERGIA:
0

CAPITULOS
NOME:             
A CHEGADA A CASA ABANDONADA
TEXTO:
Chegando na casa abandonada ,eles entram e encontram um livro chamado ,os 7 espirito amaldiçoado O que eles tem que fazer? 
PERSONAGEM:
joana
VARIAÇÃO ENERGIA:            
0

CAPITULOS
NOME:
O ENCONTRO COM O ESPIRITO 
TEXTO:
Assim que eles acabaram de ler o livro ,sete espito amaldiçoado despertam para atormentara vida daqueles que acordaram eles de seu sono profundo e entre esses sete espito está o  Hannibal,um espirito de  3 m de altura e 500 anos  de idade e que quando era umhomem foi um psicopata que matou milhares de pessoas e que  quando a policia pegou ele  torturou e matou ele nessa casa abandonada   e até hoje seu espito vive nessa casa,sendo o lider dos outros seis espirito  .Agora a missão dos três amigos é encontrar uma forma de fazer com que os 7 espiro voltem para o seu sono profundo,caso não  os espirito vão pegar , atormenta e tortura eles ate morre. Sedo  que as portas da casa abandonada se fecharam quando os espiritos depestaram , assim os tres amigos nao tem como simplemente sair da casa . como eles nao conseguia sair da casa resolveram caçar alguma soluação dentro da casa em busca de alguma solução eles acabam dando de cara com um dos espitos que acaba atacando eles e que acaba acertado ,martin, com uma paulada  fazendo com quer martin perca  energia . 
PERSONAGEM:
martin
VARIAÇÃO ENERGIA:
-10

CAPITULOS
NOME:
O LIVRAMENTO
TEXTO:
E deixando o livro quieto eles se livra do tormento dos 7 espiritos amaldiçoados.
PERSONAGEM:
joana
VARIAÇÃO ENERGIA:
-100
                   
ESCOLHAS
CAPITULO DE SAIDA:
A  TRILHA PELA FLORESTA
TEXTO:
passar direto 
CAPITULO DE CHEGADA:
DIA NORMAL

ESCOLHAS
CAPITULO DE SAIDA:
A  TRILHA PELA FLORESTA
TEXTO:
ir até a casa
CAPITULO DE CHEGADA:
A CHEGADA A CASA ABANDONADA

ESCOLHAS
CAPITULO DE SAIDA:
A CHEGADA A CASA ABANDONADA
TEXTO
eles pegam o livro e ler
CAPITULO DE CHEGADA:
O ENCONTRO COM O ESPIRITO

ESCOLHAS
CAPITULO DE SAIDA:
A CHEGADA A CASA ABANDONADA
TEXTO:
deixam o livro queto
CAPITULO DE CHEGADA:
O LIVRAMENTO     
}   



   





