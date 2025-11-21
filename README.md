// 1️⃣ REQUEST DTO (Controller riceve)
public class SearchCoachRequest
{
    public string? Name { get; set; }  // nullable
}

// 2️⃣ RESPONSE DTO (Controller ritorna)
public class SearchCoachResponse
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Surname { get; set; }
    public string Email { get; set; }
}

// 3️⃣ REPOSITORY (Data Access - Ritorna IQueryable)
public interface IRepository
{
    IQueryable<Coach> SearchCoaches(string? searchTerm);
}

public class DbRepository : IRepository
{
    private readonly GymManagerDbContext _context;

    public IQueryable<Coach> SearchCoaches(string? searchTerm)
    {
        var query = _context.Coaches.AsNoTracking();
        
        if (!string.IsNullOrWhiteSpace(searchTerm))
        {
            query = query.Where(c => c.Name.Contains(searchTerm) || 
                                      c.Surname.Contains(searchTerm));
        }
        
        return query;  // ✅ Ritorna IQueryable (non eseguito!)
    }
}

// 4️⃣ SERVICE (Business Logic)
public interface ICoachService
{
    Task<IEnumerable<SearchCoachResponse>> SearchCoaches(string? searchTerm);
}

public class CoachService : ICoachService
{
    private readonly IRepository _repo;

    public async Task<IEnumerable<SearchCoachResponse>> SearchCoaches(string? searchTerm)
    {
        // Chiama il repository (query non eseguita ancora)
        var query = _repo.SearchCoaches(searchTerm);
        
        // Esegui la query E fai il mapping
        var coaches = await query.ToListAsync();
        
        return coaches.Select(c => new SearchCoachResponse
        {
            Id = c.Id,
            Name = c.Name,
            Surname = c.Surname,
            Email = c.Email
        }).ToList();
    }
}

// 5️⃣ CONTROLLER (API Endpoint)
[HttpGet]
[Route("search")]
public async Task<ActionResult<IEnumerable<SearchCoachResponse>>> SearchCoaches(
    [FromQuery] string? name)  // ✅ Da URL: /api/coach/search?name=Marco
{
    var result = await _service.SearchCoaches(name);
    return Ok(result);
}
