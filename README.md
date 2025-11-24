# Boutique Diayma
## 5. Namespaces, classes et méthodes visités avant l’affichage des produits

Flux observé en mode debug (F10 = Step Over, F11 = Step Into, Shift+F11 = Step Out) :

1. Microsoft.AspNetCore → Program.Main() → création du host
2. Program.CreateHostBuilder() → .UseStartup<Startup>()
3. Startup() (constructeur) → breakpoint ligne 20
4. Startup.ConfigureServices() → injection de dépendances (DbContext, Repository, Session, ControllersWithViews, etc.)
5. Startup.Configure() → middleware pipeline (UseStaticFiles, UseSession, UseRouting, UseEndpoints)
6. UseEndpoints → MapControllerRoute("default", "{controller=Product}/{action=List}/{category?}")
7. Première requête GET / → routing → ProductController.List() → breakpoint ligne 15
8. ProductController.List()  
   → appel _repository.Products.OrderBy(p => p.ProductID)  
   → création du ProductsListViewModel + pagination  
   → return View(model)
9. Moteur Razor → Views/Product/List.cshtml
10. Dans List.cshtml → @await Component.InvokeAsync("CartSummary")
11. CartSummaryViewComponent.InvokeAsync() → breakpoint ligne 12  
    → récupération du panier depuis la session  
    → return View(cart)
12. Affichage final de la page d’accueil avec la liste paginée + résumé du panier

Namespaces traversés :
- Microsoft.AspNetCore.Hosting
- Microsoft.Extensions.DependencyInjection
- Microsoft.AspNetCore.Builder
- P2FixAnAppDotNetCode.Data (DbContext)
- P2FixAnAppDotNetCode.Controllers
- P2FixAnAppDotNetCode.Components
- Microsoft.EntityFrameworkCore
