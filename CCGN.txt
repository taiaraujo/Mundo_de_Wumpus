#include<stdio.h>
#include<stdlib.h>
#include<locale.h>
#include<math.h> 
#include<time.h>

void clear_pause();
int criar_amb(int **amb, int tami, int tamj, int nfos);
int jogo(int **amb, int tami, int tamj, int form, int api, int apj);
int imp_amb(int **amb, int form, int tam, int tam2);

/*
##PROJETO DE ENGENHARIA II - O MUNDO DE WUMPUS
	GRUPO:
	Cryslene Coêlho\ Gabriel Campelo\ Glaucia Caroline\ Natália Araújo
##AMBIENTE
	Temos 5 formatos de ambientes -> 1 : Quadrado \ 2: Retangular \ 3: TriÃ¢ngular \ 4: Losangular \ 5: Circular 
	Significado dos números que preenchem o ambiente ->
	 -1: agente \ 0: fora do ambiente \ 1: ouro \ 2: wumpus \ 3: fosso \ 4: flecha \ 8: vazio \ 9: escuro(o que o agente vê)
##PONTUAÇÃO
	O agente inicia o jogo na 1ª fase com 1000 pontos, se passar para a 2ª fase ganha 2000 pontos, e para 3ª 3000 pontos
	Para passar de fase o agente tem que pegar o ouro e ir para a casa inicial
	A cada movimento perde 1 ponto;
	Atirar a flecha perde 100 pontos; matar o Wumpus ganha 500 pontos;
##FLECHAS
	O agente inicia com uma flecha; a partir da 2ª fase com uma proporção de 100 casas deixamos 1 flecha disponivel no ambiente 
	O agente pode adquirir essa flecha ou o Wumpus pode quebra-la
##VIDAS
	O agente inicia com 3 vidas; 
	Perde uma vida se cair no fosso ou se na 3ª fase o agente ultrapassar o limite do ambiente
	Game over se perder todas a vidas ou se encontrar o Wumpus;
*/

int pontos, vidas, fim, flechas, fase;
int wi, wj, DAJ;

int main()
{

setlocale(LC_ALL, "");


int form, tam, tam2, i, j, x, y, ard;
int **amb, ctr=1, nfos, aux, meio, R, api, apj;
float b, b2, h;
char nome[50][50];
int pts[50], v[50], fl[50], fs[50], contf, tpg=-1, jg=0, sel;
DAJ=0;
clear_pause();
	printf("\n\t #***********************************#");
	printf("\n\t #                                   #");
	printf("\n\t #  Bem Vindo(a) ao mundo de Wumpus  #");
	printf("\n\t #                                   #");
	printf("\n\t #***********************************#");
while(DAJ!=1 && DAJ!=2){
clear_pause();
	printf("\n\t #*********************************#");
	printf("\n\t #                #                #");
	printf("\n\t #   1 - Agente   #  2 - Eu Mesmo  #");
	printf("\n\t #                #                #");
	printf("\n\t #*********************************#");
	printf("\nEscolha quem irá jogar no Mundo de Wumpus: ");
	scanf("%d", &DAJ);
}
clear_pause();
	printf("\n\t #******************************************#");
	printf("\n\t #              LEGENDA DO JOGO             #");
	printf("\n\t #------------------------------------------#");
	printf("\n\t #  -1: Agente # 1: Ouro    # 2: Wumpus     #");
	printf("\n\t #   3: Fossos # 4: Flechas # 8: Casa Vazia #");
	printf("\n\t #******************************************#");
while(tpg!=1 && tpg!=2){
	clear_pause();
	printf("\n\t #***********************************#");
	printf("\n\t #                 #                 #");
	printf("\n\t #  1 - Torneio    #  2 - Treino     #");
	printf("\n\t #                 #                 #");
	printf("\n\t #***********************************#");
	printf("\nEscolha uma modalidade: ");
	scanf("%d", &tpg);
}
do{

	clear_pause();
	printf("\n\t Informe seu nome jogador(a): ");
	scanf("%s", nome[jg]);

pontos=1000;
vidas=3;
fim=0;
flechas=1;
fase=1;
contf=0;
while(fase>=1 && fase<=3){
contf++; form=0;
	clear_pause();

	printf("\n\t #***********************************#");
	printf("\n\t #                                   #");
	printf("\n\t #     Configuração do ambiente:     #");
	printf("\n\t #                                   #");
	printf("\n\t #***********************************#");
while(form<1 || form>5){
	clear_pause();
	printf("\n\t #********************************************************************************#");
	printf("\n\t #              #                #                #                #              #");
	printf("\n\t # 1: QUADRADO  # 2: RETANGULAR  # 3: TRIANGULAR  # 4: LOSANGULAR  # 5: CIRCULAR  #");
	printf("\n\t #              #                #                #                #              #");
	printf("\n\t #********************************************************************************#");
	printf("\nEscolha um dos formatos disponíveis para o jogo: "); 
	scanf("%d", &form);
}
	switch(form)
	{
		case 1:
			printf("Informe o tamanho de um dos lados do quadrado: ");
			scanf("%d", &tam);
			nfos = (tam*tam) / (25/3);
			amb = (int **) calloc (tam+2, sizeof(int *));
			for ( i = -1; i <= tam; i++ ) {
      				amb[i] = (int*) calloc (tam+2, sizeof(int));}
 
				for( i = -1; i <= tam; i++){
					for( j = -1; j <= tam; j++){
						if(i==-1 || i==tam || j==-1 || j==tam){
							amb[i][j]=0;}
						else{
							amb[i][j] = 8;}
				}} 
				
				amb[tam-1][0] = -1; // posição do agente
				api=tam-1; apj=0;
				tam2 = tam;
				criar_amb(amb, tam, tam2, nfos);
				clear_pause();
		break;
		case 2:
			printf("Informe o tamanho da base : ");
			scanf("%d", &tam2);
			printf("Agora a altura do triângulo : ");
			scanf("%d", &tam);
			nfos = (tam*tam2) / (25/3);
			amb = (int **) calloc (tam+2, sizeof(int *));
			for ( i = -1; i <= tam; i++ ) {
      				amb[i] = (int*) calloc (tam2+2, sizeof(int));}
 
				for( i = -1; i <= tam; i++){
					for( j = -1; j <= tam2; j++){
						if(i==-1 || i==tam || j==-1 || j==tam2){
							amb[i][j]=0;}
						else{
							amb[i][j] = 8;}
				}} // ambiente vazio
				
				amb[tam-1][0] = -1; // posição do agente
				api=tam-1; apj=0;
				//sorteio dos elementos para o ambiente
				criar_amb(amb, tam, tam2, nfos);
				clear_pause();
		break;
		case 3:
			printf("Informe a altura do triângulo: ");
			scanf("%d", &tam);
			tam2 = (2*tam-1);
			nfos = ((tam2*tam)/2) / (25/3);
			amb = (int **) calloc (tam+2, sizeof(int *));
			for ( i = -1; i <= tam; i++ ) {
      				amb[i] = (int*) calloc (tam2+2, sizeof(int));}
				for(i=-1; i<=tam; i++){
					for(j=-1;j<=tam2;j++){
						amb[i][j]=0;}}
				x = 1; y = tam-1;
				for( i = 0; i < tam; i++){ 
					for( j = tam-x; j <= y; j++){
						amb[i][j] = 8;					
					}
					x++; y++; 
				} // ambiente vazio
				
				amb[tam-1][0] = -1; // posição do agente
				api=tam-1; apj=0;
				//sorteio dos elementos para o ambiente
				criar_amb(amb, tam, tam2, nfos);
				clear_pause();
		break;
		case 4:
			printf("Informe uma diagonal para o losângo: ");
			scanf("%d", &tam);
			if(tam%2==0){tam++;}
			meio = (tam-1)/2; tam2 = tam;
			nfos = ((tam*tam2) / 2) / (25/3);
			amb = (int **) calloc (tam+2, sizeof(int *));
			for ( i = -1; i <= tam; i++ ) {
      				amb[i] = (int*) calloc (tam2+2, sizeof(int));}
				for(i=-1; i<=tam; i++){
					for(j=-1;j<=tam2;j++){
						amb[i][j]=0;}}
				x = 0; y = meio;
				for( i = 0; i < tam; i++){ 
					for( j = meio-x; j <= y; j++){
						amb[i][j] = 8;					
					}
					if(i<meio){x++; y++;}
					if(i>=meio){x--; y--;} 
				} // ambiente vazio
				
				amb[tam-1][meio] = -1; // posição do agente
				api=tam-1; apj=meio;
				//sorteio dos elementos para o ambiente
				criar_amb(amb, tam, tam2, nfos);
				clear_pause();
		break;
		case 5:
			printf("informe o raio desejado: ");
			scanf("%d", &R);
			amb = (int **) calloc (2*R+2, sizeof(int *));
			for ( i = -1; i <= 2*R; i++ ) {
      				amb[i] = (int*) calloc (2*R+2, sizeof(int));}
				for(i=-1; i<=2*R; i++){
					for(j=-1;j<=2*R;j++){
						amb[i][j]=0;}}

			nfos = ((3.14159*R*R)) / (25/3);

			for (h=0.5;h<=R-0.5;h++){
				b = sqrt((R*R)-(h*h));
				ard = b;
				b2 = ard + 0.5;
				if(b>=b2){
					j = ard;	
				}else{
					j = ard-1;}
				i = h+0.5;
				for(aux=0;aux<=j;aux++){
					amb[R-i][aux+R] = 8; 	   //1º Qd
					amb[R-i][R-aux-1] = 8;     //2º Qd
		 	  		amb[i-1+R][R-aux-1] = 8;   //3º Qd
		 			amb[i-1+R][aux+R] = 8;     //4º Qd
				}
			}
			amb[2*R-1][R] = -1; //agente
			api=2*R-1; apj=R;
			tam = 2*R; tam2 = 2*R;
			criar_amb(amb, tam, tam2, nfos);
			clear_pause();
		break;
	}

	jogo(amb, tam, tam2, form, api, apj);
	pts[jg]=pontos; v[jg]=vidas; fl[jg]=flechas; fs[jg]=contf;
}
	if(tpg==1){
		printf("Outro jogador? 0 - sim / 1 - não: ");
		scanf("%d", &sel); jg++;}
	if(tpg==2){sel=0;}
}while(sel==0);
	if(tpg==1){printf("\n\tPontuação do Torneiro\n");
		for(i=0; i<jg; i++){
			printf("|Nome: %10s |Pontuação: %5d |Vidas: %d |Flechas:%d |Fase: %d|\n", nome[i], pts[i], v[i], fl[i], fs[i]);}}
return 0;
}

int criar_amb(int **amb, int tami, int tamj, int nfos)
{
	int i, j, ctr, aux, vac, vz, agt, gold, wp, fos, x, fl;
	int casa[9];

	ctr=1; fl=0;
	if(fase>=2){fl=(tami*tamj)/100;}

	srand(time(NULL)); 
	while( ctr <= nfos+fl+2){
		aux=0; vac=0, vz=0, agt=0, gold=0, wp=0, fos=0;
		i=rand()%tami;
		j=rand()%tamj;
		//posicionar elemento na posição sorteada
		if( amb[i][j] == 8 ){
			//verificar os elementos ao redor
			casa[1]=amb[i-1][j-1]; casa[2]=amb[i-1][j]; casa[3]=amb[i-1][j+1]; 
			casa[4]=amb[i][j-1];     		    casa[5]=amb[i][j+1]; 
			casa[6]=amb[i+1][j-1]; casa[7]=amb[i+1][j]; casa[8]=amb[i+1][j+1];
			for(x=1; x<=8; x++){
				if(casa[x]==-1){agt++;} if(casa[x]==0){vac++;} if(casa[x]==1){gold++;}
				if(casa[x]==2){wp++;} if(casa[x]==3){fos++;} if(casa[x]==8){vz++;}}
			//fim das verificações
			if(ctr< 3){
				if(ctr==1 && agt==0){amb[i][j] = ctr; aux++;} //posicionar ouro
				if(ctr==2){amb[i][j] = ctr; wi=i; wj=j; aux++;} //posicionar wumpus
			}
			if(ctr>=3 && ctr<=nfos+2){		
				if(fos<1){
					if(vac==2 && vac==4 && agt==0 && gold==0 && wp==0){
						amb[i][j] = 3; aux++;}
					if(vac!=2 && vac!=4){
						amb[i][j] = 3; aux++;}
			}}//posicionar fossos
			if(ctr>nfos+2){
				if(agt==0){amb[i][j] = 4; aux++;}}//posicionar flechas
			if(aux==1){ctr++;}
		}
	}
}

int jogo(int **amb, int tami, int tamj, int form, int api, int apj)
{
	struct agente{
		int p[5];
		int m, i, j;
	}*A;	
	A = (struct agente*) calloc (10000, sizeof(struct agente));

	int i, j, k, op, brisa, n, fl, x, y, gold, fedor;
	int c, b, e, d, aux, wp=0, pa, pw, wf=0;
	
	int **amb_agt;
	amb_agt = (int **) calloc (tami+2, sizeof(int *));
		for ( i = -1; i <= tami; i++ ) {
      			amb_agt[i] = (int*) calloc (tamj+2, sizeof(int));}

	for(i=-1; i<=tami; i++){
		for(j=-1; j<=tamj; j++){
			if(amb[i][j]==0){amb_agt[i][j]=0;}
			if(amb[i][j]!=0){amb_agt[i][j]=9;}
	}}
	if(fase==2){pa=2; pw=3;}
	if(fase==3){pa=1; pw=2;}
	i = api; j = apj; k=0; amb_agt[i][j]=-1; 
	if(DAJ==1){imp_amb(amb, form, tami, tamj);} if(DAJ==2){imp_amb(amb_agt, form, tami, tamj);}
	gold=0;
	while(vidas!=0 && fim!=1){wp++;
	if(fase==1 || wp<=pa){
		brisa=0; n=0; fedor=0; c=1; b=1; e=1; d=1; op=0;
		A[k].p[1] = amb[i-1][j]; A[k].p[2] = amb[i+1][j]; A[k].p[3] = amb[i][j-1]; A[k].p[4] = amb[i][j+1];	
		printf("Você está sentindo: \n");
		for(x=1; x<5; x++){
			if(A[k].p[x]==2){printf("\tFedor"); fedor++;}
			if(A[k].p[x]==3 && brisa==0){printf("\tBrisa"); brisa++;}
			if(A[k].p[x]==0 && n==0){printf("\tNada"); n++;}
			if(A[k].p[x]==8 && n==0){printf("\tNada"); n++;}}
	  //decisão e movimentação do agente
	  //antes de pegar o ouro
if(DAJ==1){
	  if(gold==0){y=0; x=0; fl=0; 
		//pelas sensasões
		//pelo ambiente conhecido pelo agente
		if(amb_agt[i-1][j]==0||amb_agt[i-1][j]==3){c=0; y++;} if(amb_agt[i+1][j]==0||amb_agt[i+1][j]==3){b=0; y++;}
		if(amb_agt[i][j-1]==0||amb_agt[i][j-1]==3){e=0; y++;} if(amb_agt[i][j+1]==0||amb_agt[i][j+1]==3){d=0; y++;}
		if(brisa==0 && fedor==0){
			//para o agente não voltar
			if(k>=1 && form!=3 && form!=4){
			if(A[k-1].m==1){b=0; y++;} if(A[k-1].m==2){c=0; y++;} 
			if(A[k-1].m==3){d=0; y++;} if(A[k-1].m==4){e=0; y++;}}
			if(amb_agt[i-1][j]==9){c++;x++;} if(amb_agt[i+1][j]==9){b++;x++;}
			if(amb_agt[i][j-1]==9){e++;x++;} if(amb_agt[i][j+1]==9){d++;x++;} 
		} 
		if(fedor==1 && flechas!=0){fl=6;}
		y=100/(4-y+x); c=y*c; b=y*b; e=y*e; d=y*d;
			if(b!=0){b=b+c; y=b;} if(b==0){y=c;}
			if(e!=0){e=e+y; y=e;} if(e==0){y=b+c;}
			if(d!=0){d=d+y;}		
		srand(time(NULL));
		x=rand()%100+1;
		if(op!=5){
		if(x<=c){op=1;} if(x>c && x<=b){op=2;} if(x>c && x>b && x<=e){op=3;} if(x>c && x>b && x>e && x<=d){op=4;}}
		if(fl==6){fl=op; op=5;}
		aux=k;
	  }
	  if(gold==1){c=0; b=1; e=1; d=1; y=0; x=0;
		if(amb_agt[i+1][j]==3){b=0;y++;} if(amb_agt[i][j-1]==3){e=0;y++;} if(amb_agt[i][j+1]==3){d=0;y++;}
		if(amb_agt[i+1][j]==0){b=0;y++;} if(amb_agt[i][j-1]==0){e=0;y++;} if(amb_agt[i][j+1]==0){d=0;y++;}
		if(amb_agt[i+1][j]==8){b++;} if(amb_agt[i][j-1]==8){e++;} if(amb_agt[i][j+1]==8){d++;}
			if(A[aux].m==1){b++;x++;} if(A[aux].m==2){c=1;x++;}
			if(A[aux].m==3){d++;x++;} if(A[aux].m==4){e++;x++;}		
  		if(j>apj){e++;x++;}
		if(j<apj){d++;x++;}
		if(j==apj){
			if(i<api){b++;x++;}	
		}
		y=100/((3+x)-y); c=y*c; b=y*b; e=y*e; d=d*e;
			if(b!=0){b=b+c; y=b;} if(b==0){y=c;} 
			if(e!=0){e=e+b; y=e;} if(e==0){y=b;}
			if(d!=0){d=d+y;}	
		x=rand()%100+1;
		if(x<=c){op=1;}if(x>c && x<=b){op=2;} if(x>c && x>b && x<=e){op=3;} if(x>c && x>b && x>e && x<=d){op=4;}
		aux--;	  
	}
}
if(DAJ==2){
	printf("\n1-cima \t2-baixo \t3-esquerda \t4-direita \t5-Atirar Flecha: ");
	scanf("%d", &op);
	if(op==5){
		printf("\n1-cima \t2-baixo \t3-esquerda \t4-direita: ");
		scanf("%d", &fl);		
	}
}	
		//op variavel de decisão
		A[k].i = i; A[k].j=j; A[k].m = op; k++;
		//jogada
		clear_pause();
		if(op!=5){amb_agt[i][j]=8; amb[i][j]=8;
			if(op==1){i--;} if(op==2){i++;} if(op==3){j--;} if(op==4){j++;}
				if(amb[i][j]==0){
					if(fase<=2){i=A[k-1].i; j=A[k-1].j; printf("Borda!");}
					if(fase==3){vidas--; i=api; j=apj;
						printf("\nVocê tentou fugir!\nPerdeu uma vida, agora você possui %d vida(s)", vidas);}}
				if(amb[i][j]==8){pontos--; amb_agt[i][j]=-1; amb[i][j]=-1;}
				if(amb[i][j]==1){pontos--; amb_agt[i][j]=-1; amb[i][j]=-1; gold=1; 
					printf("\nVocê vê Brilho\n Parabéns você pegou o ouro!\n");}
				if(amb[i][j]==2){vidas=0; printf("\nGame Over\n Você achou o Wumpus\n\a");}
				if(amb[i][j]==3){vidas--; pontos--; amb_agt[i][j]=3; i=A[k-1].i; j=A[k-1].j; amb_agt[i][j]=-1; amb[i][j]=-1;
					printf("\nVocê caiu no fosso\n Perdeu uma vida, agora possui %d vida(s)\n", vidas);}
				if(amb[i][j]==4){flechas++; pontos--; amb_agt[i][j]=-1; amb[i][j]=-1;
					printf("\nParabéns você achou uma flecha\nAgora você possui %d flecha(s)", flechas);}}
		//jogar flecha
		if(op==5){
			if(flechas==0){printf("\nVocê não possui mais flechas");}
			if(flechas>=1){pontos = pontos-100; flechas--;
			switch(fl)
			{
				case 1: /*cima*/ x=i-1; y=j; break;
				case 2: /*baixo*/ x=i+1; y=j; break;
				case 3: /*esquerda*/ x=i; y=j-1; break;
				case 4: /*direita*/ x=i; y=j+1; break;
			}
			if(amb[x][y]!=2){printf("\nVocê perdeu uma flecha\n");}
			if(amb[x][y]==2){amb[x][y]=8; amb_agt[x][y]=8; pontos=pontos+500; wf=1;
				printf("\n Você ouviu um grito\n Wumpus morreu\n Parabéns você ganhou 500 pontos");}
		}}
	}
//movimentação do Wumpus
	if(wf==1){wp=0;}
	if(fase>=2 && wp==pw){wp=0;
		c=1; b=1; e=1; d=1; y=0; x=0;
		if(amb[wi-1][wj]==-1){c++;x++;} if(amb[wi+1][wj]==-1){b++;x++;} if(amb[wi][wj-1]==-1){e++;x++;} if(amb[wi][wj+1]==-1){d++;x++;}
		if(amb[wi-1][wj]==3){c=0;y++;} if(amb[wi+1][wj]==3){b=0;y++;} if(amb[wi][wj-1]==3){e=0;y++;} if(amb[wi][wj+1]==3){d=0;y++;}
		if(amb[wi-1][wj]==0){c=0;y++;} if(amb[wi+1][wj]==0){b=0;y++;} if(amb[wi][wj-1]==0){e=0;y++;} if(amb[wi][wj+1]==0){d=0;y++;}
		y=100/((4+x)-y); c=y*c; b=y*b; e=y*e; d=d*e;
			if(b!=0){b=b+c; y=b;} if(b==0){y=c;} 
			if(e!=0){e=e+b; y=e;} if(e==0){y=b;}
			if(d!=0){d=d+y;}	
		x=rand()%100+1;
		if(x<=c){op=1;}if(x>c && x<=b){op=2;} if(x>c && x>b && x<=e){op=3;} if(x>c && x>b && x>e && x<=d){op=4;}
		printf("\n\tPasso do Wumpus");
		     clear_pause();
		if(amb[wi][wj]!=1){amb[wi][wj]=8;}
			if(op==1){wi--;} if(op==2){wi++;} if(op==3){wj--;} if(op==4){wj++;}
				if(amb[wi][wj]==8){amb[wi][wj]=2;}
				if(amb[wi][wj]==0){printf("O Wumpus caiu do ambiente"); wf=1;}
				if(amb[wi][wj]==-1){vidas=0; printf("\nGame Over\n O Wumpus te achou\n\a"); amb[wi][wj]=2;}
				if(amb[wi][wj]==3){printf("\nWumpus caiu no fosso\n"); wf=1;}
				if(amb[wi][wj]==4){flechas--; amb[wi][wj]=2; printf("\nO Wumpus quebrou uma flecha\n");}	
	}
		//fim da jogada
		if(DAJ==1){imp_amb(amb, form, tami, tamj);} if(DAJ==2){imp_amb(amb_agt, form, tami, tamj);}
		if(gold==1 && i==api && j==apj){fim=1;}
	}
	clear_pause();
	if(fim==1){fase++;
		if(fase<=3){pontos=pontos+(fase*1000);fim=0;
			    printf("\n\tParabéns passou de fase\n Pontuação: %d \tVidas: %d\tFase: %d", pontos, vidas, fase);}
		if(fase==4){printf("\n\tParabéns você ganhou o jogo\n Pontuação: %d\tVidas: %d", pontos, vidas);}}
	imp_amb(amb, form, tami, tamj);
	if(vidas==0){fase=0; printf("\tPontuação: %d\n\t vidas: %d\n", pontos, vidas);}

}

int imp_amb(int **amb, int form, int tam, int tam2)
{
int i, j, x, y, aux, meio, ctr;

	printf("\n\t Mundo de Wumpus\n\t Fase: %d\n", fase);
	switch(form)
	{
	case 1:

		for( i = 0; i < tam; i++ ){
			for( j = 0; j < tam; j++ ){
				printf("%2d", amb[i][j]); 
				}
			printf("\n"); 
		}
	
	break;
	case 2:
		for( i = 0; i < tam; i++ ){
			for( j = 0; j < tam2; j++ ){
				printf("%2d", amb[i][j]);
			}
			printf("\n");
		}
	break;
	case 3:
		x = 1; y = tam-1; aux = 1;
		for( i = 0; i < tam; i++){ 
			for( j = tam-x; j <= y; j++){
				if(j==0){aux++;}
				if(aux==1){
				for(ctr=0; ctr<j; ctr++){
					printf("  "); aux++;}}
					printf("%2d", amb[i][j]);					
				}
			x++; y++; aux=1;
		printf("\n");} 
	break;
	case 4:
		meio = (tam-1)/2; x = 0; y = meio; aux = 1;
		for( i = 0; i < tam; i++){ 
			for( j = meio-x; j <= y; j++){
				if(j==0){aux++;}
				if(aux==1){
				for(ctr=0; ctr<j; ctr++){
					printf("  "); aux++;}}
			printf("%2d", amb[i][j]);}
			aux=1;
			if(i<meio){x++; y++;}
			if(i>=meio){x--; y--;} 
			printf("\n");} 
	break;
	case 5:
		for (i=0;i<tam;i++){
			for (j=0;j<tam2;j++){
				if(amb[i][j]!=0){
					printf("%2d", amb[i][j]);}
				else{printf("  ");}}	
			printf("\n");}
	break;
	}
	printf("\nLEGENDA:\n-1: Agente\n 1: Ouro\n 2: Wumpus\n 3: Fossos\n 4: Flechas\n 8: Casa Vazia\n");
	
}

void clear_pause()
{
	#ifdef linux
		printf("\n\n Pressione enter para prosseguir...\n");
		getchar();
		system("clear");
	#else
		printf("\n");		
		system("pause");
		system("cls");
	#endif
}
