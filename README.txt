	SHIFTER:

	->No shifter, como nós criamos mais 4 novas funções, o sllv, srlv, sra e srav, tivemos de expandir os bits de fn de 2 bits para 3 bits, para que dessa forma as 8   funções do shift pudesem ser chamadas. Nessa transição, ao contrário de, a partir do registrador destino, deslocarmos o restante da instrução um bit para a direita,   escolhemos utilizar 3 dos últimos 5 bits da instrução. Dessa forma, os bits de fn se tornaram inutilizados, tornando os 3 últimos bits responsáveis por definir qual   função do shift deve ser realizada.Assim sendo, conseguimos ajustar a instrução para que reconheça os 3 bits das funções shift. 
	->A função sllv deve deslocar o conteúdo de um certo registrador uma certa quantidade de vezes que deve ser definida na instrução para a esquerda, colocando a   quantidade de zeros necessários no bit menos significativo ao longo do deslocamento para a esquerda, e os bits mais significativos são perdidos.
	->A função srlv realiza as mesmas ações que a função sllv só que desloca os bits para a direita.
	->A função sra desloca os bits para a direita mantendo o sinal, ou seja, mantendo igual o bit mais significativo.
	->A função srav faz a mesma operação que a função sra, porém ao invez de usar uma constante para determinar o deslocamento ,ela usa o valor de um registrador r2.
	->No pP-behav, nós apenas implementamos as 4 novas funções no procedure perform_shift_op para que elas realizassem oque foi explicado anteriormente.Criamos uma   porta B, para que seja utilizado nas funções que utilizam o valor de um registrador para a deslocagem de bit, pois se apenas direcionassemos a porta "sc" para o   registrador ele usaria seu endereço como um int. 
	->Para realizar o teste devesse colocar, nos 3 primeiros bits o opcode do shift que é 110, nos dois seguintes bits, coloca-se dois zeros, pois os bits de fn não   estão sendo utilizados, nos próximos nove bits devesse colocar 3 registradores na ordem: registrador destino, registrador 1 e registrador 2, então são postos mais dois   zeros (pois são os 2 dos 5 últimos bits que não usamos) e então os 3 bits que servem para chamar uma das funções do shift.
	
	



       Load/Store halfword:
	
	->No load/store halfword, para que as duas novas funções (load half/store half) pudessem ser executadas, expandimos os bits de fn de 2 para 3 bits, só que, assim    como no shifter, ao contrário de, a partir do registrador destino, deslocar toda a instrução um bit para a direita, decidimos inutilizar os bits de fn e passamos a    utilizar os 3 últimos bits do displacement para chamar as funções, reduzindo o displacement de 8 bits para 5 bits. 
	->Como não estamos mais lidando com uma constante no disp, tivemos (em pP-unpipelined_single_cycle_rtl) que trocar (na linha 184) o IR_const por IR_disp, pois    diminuimos a quantidade de bist do disp. Só que para que o precesso se realize tivemos de concatenar ao IR_disp 3 zeros, para que seja reconhecido os 8 bits do disp.
	->A ideia implementada no behav para que conseguissemos realizar primeiramente o lh(load half) foi de realizarmos um AND do conteúdo da memória com "00001111" e    jogar em um regitrador; já o sh(store half) foi implementado baseado na ideia de concatenarmos os 4 bits menos significativos do registrador destino com os 4 bits    mais significativos do conteúdo da memória onde será armazenado tal "concatenação".
	->Para realizar o teste, devesse colocar nos 3 primeiros bits o opcode da instrução da memória que é 100, logo após 2 zeros, que representam os dois bits    inutilizados de fn, nos próximos 6 bits colocasse 2 registradores na ordem: registrador destino e registrador 1, nos 5 próximos bits colocasse uma constante(de    deslocamento a partir do endereço base) e nos 3 últimos bits, a função que deve ser realizada.