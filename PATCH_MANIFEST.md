Jhadimadi surgical backend/security patch
========================================

Core changes:
- Supabase client/server fallback credentials now target krmvdvhhtidqydlfnhmm.supabase.co with the supplied publishable key.
- Added .env.local; .gitignore already excludes .env.local.
- Product media resolution/upload is canonicalized to the `products` bucket; obsolete `product-images` runtime fallback removed.
- Product image paths reject invalid order/order-item paths and fall back to /placeholder-product.svg.
- DataContext record lists start empty and do not repopulate stale local/demo product data when Supabase is empty.
- Product/banner/post deletion waits for direct Supabase DELETE success before updating UI state.
- Realtime websocket failure no longer schedules repeated reconnect loops; HTTP synchronization remains available.
- Added J-Pay wallet database migration with non-negative balance checks and atomic RPCs for add-money approval, P2P transfer, internal purchase, and withdrawal approval.
- Added authenticated wallet API controller/routes with validation and Supabase session verification.
- Added existing private profile dashboard J-Pay card/forms without creating a new dashboard.
- Gemini proxy now requires a verified Supabase session and has a per-user/IP rate limit.
- Registration no longer trusts client `isNidVerified`; server records NID verification as pending/false until server-side verification.
- Added private NID storage RLS migration.
- Existing server image upload route already contained server-side size and magic-byte checks; preserved it.
- Added final migration to remove obsolete `product-images` bucket contents/bucket.

Database migrations to apply in order:
- supabase/migrations/20260920_jpay_wallet_atomic.sql
- supabase/migrations/20260920_private_nid_storage_rls.sql
- supabase/migrations/20260920_remove_product_images_bucket.sql

Note:
- Full npm dependency installation/build could not be completed in this environment because npm install timed out and node_modules was unavailable. TypeScript parsing was checked with the system TypeScript compiler; remaining diagnostics are dependency/type-environment related rather than syntax errors in the changed files.
