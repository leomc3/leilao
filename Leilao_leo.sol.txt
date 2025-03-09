// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Leilao {
    address public dono;
    address public maiorLicitante;
    uint public maiorLance;
    mapping(address => uint) public lances;
    bool finalizado;
    
    constructor() {
        dono = msg.sender;
    }
    
    function fazerLance() public payable {
        require(msg.value > maiorLance, "Lance deve ser maior que o atual.");
        
        // Reembolsa o antigo maior licitante
        if (maiorLicitante != address(0)) {
            payable(maiorLicitante).transfer(maiorLance);
        }
        
        lances[msg.sender] += msg.value;
        maiorLicitante = msg.sender;
        maiorLance = msg.value;
    }
    
    function finalizarLeilao() public {
        require(msg.sender == dono, "Somente o dono pode finalizar o leilao.");
        require(!finalizado, "Leilao ja foi finalizado.");
        
        finalizado = true; // Atualiza estado antes da transferência
        
        (bool sucesso, ) = payable(dono).call{value: maiorLance}("");
        require(sucesso, "Falha ao transferir fundos.");
    }
    
    function retirarLance() public {
        require(msg.sender != maiorLicitante, "Maior licitante nao pode retirar agora.");
        uint valor = lances[msg.sender];
        require(valor > 0, "Nenhum valor para retirar.");
        
        lances[msg.sender] = 0; // Atualiza antes da transferência
        (bool sucesso, ) = payable(msg.sender).call{value: valor}("");
        require(sucesso, "Falha ao transferir fundos.");
    }
}
