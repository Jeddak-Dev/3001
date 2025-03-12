const express = require('express');

const { Pool } = require('pg');

const app = express();

app.use(express.json());

const pool = new Pool({

    user: 'postgres',
    
    host: 'localhost',
    
    database: 'usuarios_db',
    
    password: '40028922',
    
    port: 5432,
    
});

// Rota para listar usuários

app.get('/usuarios', async (req, res) => {

    const result = await pool.query('SELECT * FROM usuarios;');
    
    res.json(result.rows);
    
});

// Rota para adicionar usuário

app.post('/usuarios', async (req, res) => {

    const { nome, email, idade } = req.body;
    
    await pool.query('INSERT INTO usuarios (nome, email, idade) VALUES ($1, $2, $3)', [nome, email, idade]);
    
    res.json({ message: "Usuário cadastrado com sucesso!" });
    
});

app.listen(3000, () => console.log("Servidor rodando na porta 3000"));
