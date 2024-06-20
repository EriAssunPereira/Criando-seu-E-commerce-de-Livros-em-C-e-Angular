# Criando-seu-E-commerce-de-Livros-em-C#-e-Angular

Desenvolver um e-commerce de livros utilizando C# para o Back-End e Angular para o Front-End é um projeto que envolve diversas etapas e conceitos importantes de programação. Vamos dividir o projeto em módulos principais e dar exemplos básicos de como podemos começar cada parte.

### Módulos do Projeto

1. **Back-End em C#**
   - Criação de uma API RESTful para gerenciar produtos (livros), pedidos e usuários.
   - Utilização de Entity Framework Core para mapeamento objeto-relacional (ORM).
   - Implementação de autenticação JWT (JSON Web Token) para segurança.
   - Validação de dados utilizando Data Annotations.

   Exemplo de código (Controller em C# utilizando ASP.NET Core):

   ```csharp
   [Route("api/[controller]")]
   [ApiController]
   public class LivrosController : ControllerBase
   {
       private readonly ApplicationDbContext _context;

       public LivrosController(ApplicationDbContext context)
       {
           _context = context;
       }

       // GET: api/Livros
       [HttpGet]
       public async Task<ActionResult<IEnumerable<Livro>>> GetLivros()
       {
           return await _context.Livros.ToListAsync();
       }

       // GET: api/Livros/5
       [HttpGet("{id}")]
       public async Task<ActionResult<Livro>> GetLivro(int id)
       {
           var livro = await _context.Livros.FindAsync(id);

           if (livro == null)
           {
               return NotFound();
           }

           return livro;
       }

       // POST: api/Livros
       [HttpPost]
       public async Task<ActionResult<Livro>> PostLivro(Livro livro)
       {
           _context.Livros.Add(livro);
           await _context.SaveChangesAsync();

           return CreatedAtAction(nameof(GetLivro), new { id = livro.Id }, livro);
       }
       
       // PUT: api/Livros/5
       [HttpPut("{id}")]
       public async Task<IActionResult> PutLivro(int id, Livro livro)
       {
           if (id != livro.Id)
           {
               return BadRequest();
           }

           _context.Entry(livro).State = EntityState.Modified;

           try
           {
               await _context.SaveChangesAsync();
           }
           catch (DbUpdateConcurrencyException)
           {
               if (!LivroExists(id))
               {
                   return NotFound();
               }
               else
               {
                   throw;
               }
           }

           return NoContent();
       }

       // DELETE: api/Livros/5
       [HttpDelete("{id}")]
       public async Task<IActionResult> DeleteLivro(int id)
       {
           var livro = await _context.Livros.FindAsync(id);
           if (livro == null)
           {
               return NotFound();
           }

           _context.Livros.Remove(livro);
           await _context.SaveChangesAsync();

           return NoContent();
       }

       private bool LivroExists(int id)
       {
           return _context.Livros.Any(e => e.Id == id);
       }
   }
   ```

2. **Front-End em Angular**
   - Implementação de componentes para listar livros, exibir detalhes do livro, carrinho de compras, etc.
   - Integração com a API Back-End utilizando serviços HTTP do Angular.
   - Utilização de rotas para navegação entre páginas.

   Exemplo de código (Componente em TypeScript para listar livros):

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { Livro } from '../models/livro';
   import { LivroService } from '../services/livro.service';

   @Component({
     selector: 'app-lista-livros',
     templateUrl: './lista-livros.component.html',
     styleUrls: ['./lista-livros.component.css']
   })
   export class ListaLivrosComponent implements OnInit {
     livros: Livro[];

     constructor(private livroService: LivroService) { }

     ngOnInit(): void {
       this.carregarLivros();
     }

     carregarLivros() {
       this.livroService.getLivros().subscribe(
         (data) => {
           this.livros = data;
         },
         (error) => {
           console.error('Erro ao carregar livros', error);
         }
       );
     }
   }
   ```

   Exemplo de HTML associado ao componente:

   ```html
   <ul>
     <li *ngFor="let livro of livros">
       {{ livro.titulo }} - {{ livro.autor }}
     </li>
   </ul>
   ```

3. **Segurança e Autenticação**
   - Implementação de login e registro de usuários.
   - Proteção de rotas no Angular com guarda de rotas.
   - Uso de tokens JWT para autenticação com a API.

4. **Interface de Usuário (UI/UX)**
   - Utilização de componentes Angular Material ou Bootstrap para uma interface amigável.
   - Implementação de funcionalidades como pesquisa de livros, adição ao carrinho, checkout, etc.

5. **Persistência de Dados**
   - Utilização de banco de dados SQL Server, MySQL ou SQLite com Entity Framework Core no Back-End.
   - Armazenamento de informações de usuário e histórico de pedidos.

Este projeto é complexo e envolve muitos detalhes adicionais, como validação de formulários, tratamento de erros, internacionalização, entre outros. Certifique-se de estudar cada tecnologia envolvida (C#, ASP.NET Core, Angular) e adaptar o projeto de acordo com suas necessidades e requisitos específicos.
