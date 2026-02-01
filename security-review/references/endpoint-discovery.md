# Endpoint Discovery Guide

Use this as a quick reference for locating exposed endpoints and the middleware that protects them. Prefer `rg -n` searches so you can attach file+line evidence.

## Common search patterns
- Express/Koa/Fastify/Hapi: `app.get`, `app.post`, `router.get`, `router.post`, `fastify.get`, `fastify.route`, `server.route`
- NestJS: `@Controller`, `@Get`, `@Post`, `@Put`, `@Patch`, `@Delete`, `@UseGuards`
- FastAPI/Starlette: `@app.get`, `@app.post`, `APIRouter`, `add_api_route`, `Depends`
- Flask: `@app.route`, `Blueprint`, `add_url_rule`
- Django: `urlpatterns`, `path(`, `re_path(`, `include(`, class-based views
- Rails: `config/routes.rb`, `resources`, `get`, `post`, `namespace`, `member`, `collection`
- Spring: `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PreAuthorize`
- ASP.NET: `[HttpGet]`, `[HttpPost]`, `MapGet`, `MapPost`, `MapControllers`
- Go: `http.HandleFunc`, `mux.HandleFunc`, `router.Handle`, `gin.GET`, `gin.POST`
- GraphQL: `/graphql` route and resolvers; treat sensitive resolver groups as endpoints
- gRPC: `.proto` service definitions; treat each RPC method as an exposed endpoint
- WebSockets: `ws.on`, `io.on`, `@SubscribeMessage`, `WebSocketGateway`

## Auth and rate limiting discovery
Look for middleware/guards or decorators near routes or applied globally:
- Auth: `auth`, `requireAuth`, `authenticate`, `authorize`, `jwt`, `passport`, `session`, `@UseGuards`, `@PreAuthorize`, `Depends(current_user)`
- Rate limiting: `rateLimit`, `throttle`, `express-rate-limit`, `@Throttle`, `Rack::Attack`, `SlowAPI`, `bucket4j`, `resilience4j`

## Notes
- If route registration is centralized, find the router import chain to avoid missing nested routes.
- If endpoints are generated from OpenAPI or gRPC, use the spec files as the source of truth.
