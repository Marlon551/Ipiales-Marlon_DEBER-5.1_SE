# Ipiales-Marlon_DEBER-5.1_SE
Ipiales Marlon_DEBER 5.1_SE
/*
*CAPITULO V: PERIFERICOS ESPECIFICAS
*DEBER 5.1: Lectura y escritura en la memoria EEPROM
*OBJETIVO: Almacenar un valor de conversor analogo digital si se encuentra dentro de un rango establecido.
*NOMBRE: Marlon Ipiales
*FUNCIONES: EEPROM.write(dir,valor)
*           EEPROM.read(dir)
*           EEPROM.update(dir,valor)
*           dir -> 0,1023
*           valor -> 8bits  
*NOTA: Si no funcionara la compilacion escoger arduino uno WIFI caso contrario solo arduino uno, En caso de  no compilar correctamente guarde el archivo y compile varias veces */
#include <Keypad.h>
#include <LiquidCrystal.h>   
#include <EEPROM.h>
const byte rows = 4; 
const byte col = 4;
const int POT = A3; 
int value,PW,CurrentPW=1234;
float volt;
char Key;
byte pRows[]  = {3,2,1,0};
byte pCol[] = {7,6,5,4};
char teclas[4][4] = {{'7','8','9','A'},
{'4','5','6','B'},
{'1','2','3','C'},
{'*','0','=','D'}};
Keypad teclado = Keypad( makeKeymap(teclas), pRows, pCol, rows, col); 
LiquidCrystal lcd(13, 12, 11, 10, 9, 8); 
void setup() {
lcd.begin(16, 2);   
teclado.setHoldTime(1000); 
EEPROM.get( 0, PW );
if(PW!=CurrentPW && PW>0){
CurrentPW=PW;
}

}
int password(void){
int pass,i=0;
String KeyWord;

lcd.setCursor(0, 1);
while(i<4){
Key=teclado.waitForKey(); 
if(Key>='0' && Key<='9'){
lcd.println(KeyWord);
KeyWord += Key;   
i++;
}
if(Key=='D'){ 
i=4;
}
}
if(KeyWord.length()>0){
pass=KeyWord.toInt(); 
}else{
pass=0;
}
return(pass);
}
void loop() {
char KeyOp;
int i,j;
float vEE[4];


lcd.home();         
lcd.print("BIENVENIDO");   
lcd.setCursor(0,1);        
lcd.print("Press / to continue");  
Key=teclado.getKey();
if(Key>='A' && Key<='C'){
KeyOp=Key;
lcd.clear();
lcd.home();         // Coloca el cursor en las coordenadas (0,0)   
lcd.print("Digite clave"); 
PW=password();
//Valida a senha
if(PW == CurrentPW){
  lcd.print("HOLA"); 




}else{
lcd.clear();
lcd.home();         // Coloca el cursor en las coordenadas (0,0)   
lcd.print("Clave error");  
delay(2000); 
}
} 
}
