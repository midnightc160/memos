# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Backend (Go)
- **Build**: `./scripts/build.sh` - Builds the Go binary to `./build/memos`
- **Run**: `go run ./cmd/memos` - Run the main application directly
- **Test**: `go test ./...` - Run all Go tests
- **Test specific package**: `go test ./internal/util` - Run tests for a specific package

### Frontend (React/TypeScript)
All frontend commands should be run from the `web/` directory:

- **Development**: `pnpm dev` - Start development server with hot reload
- **Build**: `pnpm build` - Build for development
- **Production build**: `pnpm release` - Build for production (outputs to `../server/router/frontend/dist`)
- **Lint**: `pnpm lint` - Run TypeScript and Biome checks
- **Lint fix**: `pnpm lint:fix` - Auto-fix lint issues with Biome
- **Format**: `pnpm format` - Format code with Biome

## Architecture

### Overall Structure
Memos is a full-stack application with:
- **Backend**: Go-based REST/gRPC API server using Echo framework
- **Frontend**: React SPA with TypeScript, built with Vite
- **Database**: SQLite, MySQL, or PostgreSQL support via Go database/sql
- **Storage**: Local filesystem or S3-compatible object storage

### Key Directories

#### Backend (`/`)
- `cmd/memos/` - Main application entry point
- `server/` - HTTP server, API routes, and business logic
  - `router/api/v1/` - REST API handlers and Connect services
  - `auth/` - Authentication and authorization middleware
- `store/` - Database layer with interfaces and implementations
- `plugin/` - Modular features (email, webhooks, markdown parsing, etc.)
- `internal/` - Internal utilities and base types
- `scripts/` - Build and deployment scripts

#### Frontend (`web/`)
- `src/` - React application source
- Built with Vite, uses pnpm for package management
- Uses Biome for linting and formatting
- Outputs to `server/router/frontend/dist/` for production builds

### Database Architecture
The application uses a store pattern with interfaces in `store/` and implementations for different database drivers. Tests are located in `store/test/` and use testcontainers for integration testing.

### API Design
- REST API endpoints under `/api/v1/`
- Connect-RPC services for type-safe gRPC communication
- Authentication via JWT tokens
- Resource-based URL structure

### Testing
- Go: Standard `testing` package with testcontainers for integration tests
- Frontend: Tests should be run via `pnpm test` (check package.json for exact command)
- Test files follow Go conventions (`*_test.go`) and are co-located with source