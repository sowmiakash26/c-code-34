#include<stdio.h>
char board[3][3] = {{' ',' ',' '},{' ',' ',' '},{' ',' ',' '}};
void print_board(){
   printf("%c | %c | %c\n",board[0][0],board[0][1],board[0][2]);
   printf("----------\n");
   printf("%c | %c | %c\n",board[1][0],board[1][1],board[1][2]);
   printf("----------\n");
   printf("%c | %c | %c\n",board[2][0],board[2][1],board[2][2]);
}
void get_markers(char *m1, char *m2){
   char marker1;
   printf("Player 1 choice (X or O) : ");
   scanf("%c",&marker1);
   if(marker1=='X' || marker1=='x'){
      *m1='X';
      *m2='O';
   }else if(marker1=='O' || marker1=='o'){
      *m1='O';
      *m2='X';
   }else{
      printf("Invalid Input");
      get_markers(m1,m2);
   }
}
int get_coordinates(int *x, int *y){
   printf("Enter the coordinates (row and column): ");
   scanf("%d %d",x,y);
   if(*x < 0 || *x > 2 || *y < 0 || *y > 2 || board[*x][*y] != ' '){
     printf("Invalid input or position already taken. Try again.\n");
   }
}
int check_for_win(){
   for(int i=0;i<3;i++){
      if(board[i][0]==board[i][1] && board[i][1] == board[i][2] && board[i][1] != ' ')
         return 1;
   }
   for(int i=0;i<3;i++){
      if(board[0][i]==board[1][i] && board[1][i] == board[2][i] && board[1][i] != ' ')
         return 1;
   }
   if(board[0][0]==board[1][1] && board[1][1] == board[2][2] && board[1][1] != ' '){
         return 1;
   }
   if(board[0][2]==board[1][1] && board[1][1] == board[2][0] && board[1][1] != ' '){
         return 1;
   }
   return 0;
}
int update_board(int chance, char marker, int x, int y){
   board[x][y] = marker;
   if(check_for_win()){
     return 1;
   }
   return 0;
}
void play_game(){
   int chance = 1;
   char m1,m2;
   get_markers(&m1,&m2);
   printf("Player 1 : %c, Player 2 : %c\n",m1,m2);
   for(int turns=0;turns<9;turns++){
     print_board();
     int x,y;
     get_coordinates(&x,&y);
     if(chance){
        if(update_board(chance,m1,x,y)){
            print_board();
            printf("Player 1 wins!\n");
            break;
        }
        chance = 0;
     }
     else{
        if(update_board(chance,m2,x,y)){
            print_board();
            printf("Player 2 wins!\n");
            break;
        }
        chance = 1;
     }
   }
   if(check_for_win() == 0 && chance == 0){
     printf("It's a draw!\n");
   }
}
int main(){
   play_game();
   return 0;
}
