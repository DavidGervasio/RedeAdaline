import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;


public class Adaline {

	public int quatidadeDeEntradas = 5;//Obs essa variavel deve ser visivel para classe Amostra (considerar entrada do peso do limiar)
	
	
	public  class Amostra{
			
		private double vetorDeEntradas[] = new double[quatidadeDeEntradas];//Obs: w0 w1 w2 ...  onde w0 e sempre -1 para correção do limiar
		private double saida;
		
		public double getSaida() {
			return saida;
		}
		public void setSaida(double saida) {
			this.saida = saida;
		}
		
	}//Fim classe Amostra
	


	
	//Preencher um array do tipo amostra utilizando arquivo
	public void preencherArrayDeAmostras(  ArrayList<Amostra>arrayListAmostra, String nomeDoArquivo ){
		try {
				FileReader arq = new FileReader("/home/davidg/Documentos/ProgramasJava/ProgramasComEclip/RedeAdaline/src/"+nomeDoArquivo);
				BufferedReader lerArq = new BufferedReader(arq);
				String linha = lerArq.readLine(); 
				linha = lerArq.readLine();//Pular primeira linha		
				
				while (linha != null) {
					
					String[] vetorDeString = null;
					String[] removeLinhaBranca; //Remove da string parte nula da linha
					removeLinhaBranca = linha.split("      ");//Fim dos dados linha branca maior que 5espaços
					vetorDeString = removeLinhaBranca[0].split("   ");	//Divisão 3 espaços em brancos
					Amostra aux = new Amostra();
						
					
					for(int i=0; i<quatidadeDeEntradas; i++){
						aux.vetorDeEntradas[i] =  Double.parseDouble(vetorDeString[i] ) ;
					}
					
					aux.setSaida(Double.parseDouble(vetorDeString[quatidadeDeEntradas] ) );
					
					arrayListAmostra.add(aux);
					linha = lerArq.readLine(); // lê da segunda até a última linha
				}

					arq.close();
				}catch (IOException e) {
						System.err.printf("Erro na abertura do arquivo: %s.\n",
				    	e.getMessage());
				}
	}//Fim PreencherArrayDeDados
	
	
	
	
	
	public double potencialDeAtivacao(double vetorDeEntradas[],double vetorDePesos[] ){
		double soma = 0;
		
		for (int i = 0;i < vetorDeEntradas.length; i++){
			soma = soma + (vetorDeEntradas[i] * vetorDePesos[i]);
		}
		return soma; 
	}
	
	
	
	
	public int funcaoDeAtivacao(double potencialDeAtivacao){
		
		if(potencialDeAtivacao > 0 || potencialDeAtivacao == 0  ){
			return 1;
		}else{
			return -1;
		}
		
	};
	
	
	
	
	public void treinamento( double vetorDePesos[] ){
		double taxaDeAprendizagem = 0.001, erroQuadraticoMedioAnterior = 0, erroQudraticomedioatual  = 0 , precisao = 0.00000001, resultadoEncontrado = 0  ;  
		int epocas = 0,  resultadoEsperado;
		ArrayList< Amostra > arrayListTodasAmostra= new ArrayList< Amostra >();
		
		preencherArrayDeAmostras( arrayListTodasAmostra, "Amostras.txt" );
		
		do{
			erroQuadraticoMedioAnterior = erroQuadraticoMedio( arrayListTodasAmostra , vetorDePesos );
			for(int i = 0; i< arrayListTodasAmostra.size(); i++ ){
					
				resultadoEncontrado =  potencialDeAtivacao( arrayListTodasAmostra.get(i).vetorDeEntradas, vetorDePesos ) ;
				resultadoEsperado = (int) arrayListTodasAmostra.get(i).saida ;
				
				if(resultadoEncontrado != resultadoEsperado ){
					for(int j = 0; j< quatidadeDeEntradas; j++){ //Reajuste dos pesos e do limiar 
						 vetorDePesos[j] =  vetorDePesos[j] + ( taxaDeAprendizagem * (  (  resultadoEsperado - resultadoEncontrado ) *  arrayListTodasAmostra.get(i).vetorDeEntradas[j] ) );
						
					}
				}
			};
			
			epocas ++; 			 
			erroQudraticomedioatual =  erroQuadraticoMedio( arrayListTodasAmostra , vetorDePesos );
			
		}while(Math.abs( erroQudraticomedioatual - erroQuadraticoMedioAnterior ) >  precisao ) ;
		
	/*
		System.out.println(arrayListTodasAmostra.size());
		for(int i=0;  i<arrayListTodasAmostra.size(); i++){
			System.out.println(arrayListTodasAmostra.get(i).vetorDeEntradas[0]+" "+arrayListTodasAmostra.get(i).vetorDeEntradas[1]+" "+arrayListTodasAmostra.get(i).vetorDeEntradas[2]+" "+arrayListTodasAmostra.get(i).vetorDeEntradas[3] );
		}
	*/	
		System.out.print( " EPOCA: "+ epocas);
		System.out.print(" LIMIAR: "+ vetorDePesos[0] );
		
		for(int i =2 ; i<quatidadeDeEntradas; i++) {
			
			System.out.print( " PESO "+ i +": "+ vetorDePesos[i] );
		};
		System.out.println();
		System.out.println();
	};
	
	
	public double erroQuadraticoMedio( ArrayList<Amostra>arrayListAmostra , double vetorDePesos[] ){
		
		double erroQuadraticoMedio = 0, potAtivacao = 0;
		int quantidadeDeAmostras = arrayListAmostra.size();
		
		for( int i =0; i< quantidadeDeAmostras; i++ ){
			potAtivacao = potencialDeAtivacao(arrayListAmostra.get(i).vetorDeEntradas, vetorDePesos );
			erroQuadraticoMedio = erroQuadraticoMedio + Math.pow( ( arrayListAmostra.get(i).getSaida() - potAtivacao ), 2);
		}
		return  erroQuadraticoMedio/quantidadeDeAmostras;
	}
	
	
	
	public void operacao( double vetorDePesos[] ){
		ArrayList< Amostra > arrayListTodasAmostra= new ArrayList< Amostra >();
		preencherArrayDeAmostras( arrayListTodasAmostra, "AmostrasParaClassificar.txt" );
		int saida;
		
		for( int i =0; i< arrayListTodasAmostra.size(); i++ ){
			saida = funcaoDeAtivacao( potencialDeAtivacao( arrayListTodasAmostra.get(i).vetorDeEntradas, vetorDePesos) );
			
			for(int j =2 ; j<quatidadeDeEntradas; j++) {
				System.out.print( arrayListTodasAmostra.get(i).vetorDeEntradas[j]+ "  " );
			};
			if(saida == -1){
				System.out.print(" A ");
			}else{
				System.out.print(" B ");
			}
			System.out.println();
		}
	
	}
	
	
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Adaline adaline = new Adaline();
		double vetorDePesos[] = { 0.1, 0.001, 0.03, 0.03 , 0.04};  
		adaline.treinamento( vetorDePesos );
		adaline.operacao(vetorDePesos);
	
	}
		
}


