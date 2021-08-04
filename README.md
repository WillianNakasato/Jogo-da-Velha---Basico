# Jogo-da-Velha---Basico
Jogo da Velha programado com códigos básicos de Python
#Criando o tabuleiro----------------------------------------------------------------------------------

from IPython.display import clear_output

def display_board(board):

    clear_output()

    print('   |   |   ')
    print(' '+ board[7]+' | '+board[8]+' | '+board[9])
    print('   |   |   ')
    print('-------------')
    print('   |   |   ')
    print(' '+ board[4]+' | '+board[5]+' | '+board[6])
    print('   |   |   ')
    print('-------------')
    print('   |   |   ')
    print(' '+ board[1]+' | '+board[2]+' | '+board[3])
    print('   |   |   ')
    
#Selecionando o primeiro jogador-----------------------------------------------------------------------

def player_input():
    
    marker = ''
    while not (marker == 'X' or marker == 'O'):
        marker = input('Player 1: Você quer ser X ou O?').upper()
        
    if marker == 'X':
        return ('X', 'O')
    else:
        return ('O', 'X')

#Função Posição do jogador----------------------------------------------------------------------------

def place_marker(board, marker, position):
    board[position] = marker

#Possíveis jogadas vencedoras-------------------------------------------------------------------------

def win_check(board,mark):
    
    return ((board[7] == mark and board[8] == mark and board[9] == mark) or # vitória pelo topo
    (board[4] == mark and board[5] == mark and board[6] == mark) or # pelo meio
    (board[1] == mark and board[2] == mark and board[3] == mark) or # por baixo
    (board[7] == mark and board[4] == mark and board[1] == mark) or # pela esquda
    (board[8] == mark and board[5] == mark and board[2] == mark) or # pelo meio
    (board[9] == mark and board[6] == mark and board[3] == mark) or # pela direita
    (board[7] == mark and board[5] == mark and board[3] == mark) or # diagonal
    (board[9] == mark and board[5] == mark and board[1] == mark)) # diagonal
    
#Escolhendo o primeiro jogador de forma aleatória-----------------------------------------------------

import random
def choose_first():
    if random.randint(0, 1) == 0:
        return 'Player 2'
    else:
        return 'Player 1'
        
#Verifica se o campo está livre para jogar (Se o campo estiver vazio retorna 'Verdadeiro')-----------

def space_check(board, position):
    return board[position] == ' '
    
#Verifica se o tabuleiro está preenchido (Se o campo estiver preenchido, retorna 'Verdadeiro')-------
def full_board_check(board):
    for i in range(1, 10):
        if space_check(board, i):
            return False
    
    return True
    
    
#Destacando todos os campos que são válidos para o game----------------------------------------------

def player_choice(board):
    position = ' '
    while position not in '1 2 3 4 5 6 7 8 9'.split() or not space_check(board, int(position)):
        
        position = input('Escolha sua jogada (1 a 9) ')
        return int(position)

#Mensagem de reinício ao jogador----------------------------------------------

def replay():
    return input('Quer jogar novamente? "SIM" ou "NAO"'.lower().startswith('s'))
    

#----------------------------------------------Finalmente o jogo utilizando todas as funções criadas acima----------------------------------------------
#-------------------------------------------------------------------------------------------------------------------------------------------------------
print('Bem vindo ao jogo da velha')

while True:
    board = [' '] * 10
    #Desempacotando a função tupla if marker == 'X':     
    player1_marker, player2_marker = player_input()
    turn = choose_first()
    print(turn+' começa!')
    
    game_on = True
    
    while game_on:
        #Vez do jogador 1
        if turn == 'Player 1':
            display_board(board)
            position = player_choice(board)
            place_marker(board, player1_marker, position)
            
    #Verifica se já ganhou
        if win_check(board, player1_marker):
            display_board(board)
            print('Parabéns! Você venceu!')
            game_on = False
        else:
            if full_board_check(board):
                display_board(board)
                print('Empate!')
                break
            else:
                turn = 'Player 2'
                

#-----------------------------------------------------------------------------------------------        
        #Vez do jogador 2
    
        if turn == 'Player 2':
            display_board(board)
            position = player_choice(board)
            place_marker(board, player2_marker, position)
            
    #Verifica se já ganhou
        if win_check(board, player2_marker):
            display_board(board)
            print('Parabéns! Você venceu!')
            game_on = False
        else:
            if full_board_check(board):
                display_board(board)
                print('Empate!')
                break
            else:
                turn = 'Player 1'
                    
                
    if not replay():
        break
