import java.io.*;
import java.util.*;
class Main {
  public static void main(String[] args) throws IOException{
  List custN = new ArrayList<Integer>(); 
  ArrayList nameC = new ArrayList<String>();
  List<Integer> amountN = new ArrayList<Integer>();
  List<Double> total = new ArrayList<Double>(20);

  String lineC;
  String lineT;
  int num;
  int amount;
  int quan;
  double pri;
  double tot;
  int am;
  double now;
  double Oam;
  String type;
  
  //Created read in for customer
  File file = new File("customer");
  Scanner reader = new Scanner(file);
  //Catching extceptions
  try{
  //Using while to read in values for arrays
  while(reader.hasNext()){
  int i = 0;
  lineC = reader.nextLine();
  String [] splitted = lineC.split("\\s+");
  num = Integer.parseInt(splitted[0]);
  amount = Integer.parseInt(splitted[2]);
  String name = splitted[1];
  nameC.add(name);
  double amD = (double)amount;
  total.add(amD);
  custN.add(num);
  
  amountN.add(amount);
  
  
  }
  
  
  //read in file for transaction
  File trans = new File("transaction");
  Scanner calc = new Scanner(trans);
  
  //use while loop to go through transactions
  while(calc.hasNext()){
  //read in transaction line and then split
  lineT = calc.nextLine();
  String[] splice = lineT.split("\\s+");
  type = splice[0];
  int account = Integer.parseInt(splice[1]);
  //loop through arrays to compare to current price value
  for(int i = 0; i<custN.size();i++){
    if(type.equals("O")){      
        int custInt = (int)custN.get(i);
        int current = amountN.get(i);
        if(custInt==account){
          if(current==total.get(i)){
          quan = Integer.parseInt(splice[4]);
          pri = Double.parseDouble(splice[5]);
          tot = quan*pri;
          am = amountN.get(i);
          Oam = am+tot;
         
          total.set(i,(Oam));
          
          
          
          
          }
          else{
          quan = Integer.parseInt(splice[4]);
          pri = Double.parseDouble(splice[5]);
          tot = quan*pri;
          now = total.get(i);
          Oam = now+tot;
          
          total.set(i,Oam);
        
        
          }
    
        }
      
    }
    else if(type.equals("P")) {
        int custInt = (int)custN.get(i);
        int current = amountN.get(i);
        if(custInt==account){
          if(current==total.get(i)){
          tot = Double.parseDouble(splice[4]);
          am = amountN.get(i);
          Oam = am - tot;
          total.set(i,Oam);
          
          }
          else{
          
          tot = Double.parseDouble(splice[4]);;
          now = total.get(i);
          Oam = now-tot;
          
          total.set(i,Oam);
          
          }
        }
      }
    
        }
  }
  PrintWriter fwrite = new PrintWriter("result.txt");
  

  for(int j = 0;j<amountN.size();j++){

  fwrite.println(nameC.get(j) + " " + custN.get(j) +   
   " " + amountN.get(j) + " " + total.get(j));
}
fwrite.close();
  }
catch(FileNotFoundException e){
  for(int i = 0;i<amountN.size();i++){
    int compare = amountN.get(i);
    for(;i<amountN.size();i++){
      if(compare == amountN.get(i)){
        System.out.println("There is a duplicate");
      }
    }
  }
}
}

  }
