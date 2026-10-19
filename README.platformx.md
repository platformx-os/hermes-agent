# PlatformX source integration

This fork prepares a pinned upstream source contract for a fresh, separately provisioned service. It imports no legacy settings, identities, conversations, memory or runtime patches. No image has been built or deployed by this preparation.

The authoritative selected release, upstream commit, build inputs, runtime interfaces and evidence links are in [`.platformx/source.json`](.platformx/source.json). The upstream Dockerfile and license remain unchanged.

## Check

Use Python 3.11 or newer, with Git available:

```sh
python3 .platformx/check_source.py
python3 -m unittest discover -s .platformx -p 'test_source.py'
git diff --check
```

These offline checks validate immutable identity shape and ancestry, Dockerfile/license hashes, image entrypoint/default command, provenance ownership, version metadata and rejection boundaries. They do not import the application, install dependencies, open application state or contact a model or messaging service. Sparse checkouts can run these checks; a real image build needs full tracked source at its reviewed integration commit.

## Build and runtime contract

Reuse `build.dockerfile` with `build.context`. Apply declared preparation argument arrays and build arguments in a future reviewed build; replace build-identity placeholders with verified values, never shell-evaluate them. Set the required OCI labels during the build. `BUILD_SOURCE_COMMIT` means the actual custom integration commit; `upstream_commit` names its selected upstream baseline. Network/package-manager build steps mean byte-for-byte rebuild reproducibility is not certified.

Hermes starts the upstream dispatcher as root for s6 initialization; its command wrapper drops to the hermes service account. Preserve its default state mount and entrypoint. Initialization can modify state, permissions and API credentials. A future image build must run the upstream install-stamp preparation recorded in build.prepare with its actual integration commit. Generic project/package versions do not prove runtime release identity.

Private infrastructure owns named volumes, new bot credentials, provider setup, mounts, network policy, approved image digests, logging/cache budgets and activation. Container paths in the contract are upstream interfaces, not host paths. No host-wide mounts or container socket are requested here.

Shared rules, skills and memory should be operator-provided read-only resources, integrated through selected upstream workspace/instruction mechanisms. A mounted document alone does not prove that an agent reads it; demonstrate instruction loading during later acceptance. Authored workspace data and native thread state must be distinguished from generated package/index caches.

## Updates and rollback

`main` is an unchanged upstream mirror. Reviewed integration changes belong on `platformx/main`; one change uses a `platformx/<change>` PR into that branch. No legacy branch is force-synced. A future private updater may open a draft stable-release PR, but must refresh this source contract, review conflicts and Docker/runtime/version changes, and pass compatibility checks before merge. No auto-merge or auto-deploy is configured here.

Repository Actions remain disabled during preparation. The JSON records this preparation boundary; it does not enforce repository settings. Keep inherited workflows disabled until a separately reviewed build path and its cost/security boundaries are approved. This public fork contains portable source metadata only; deployment settings and credentials belong in private infrastructure.

Rollback for this source-only preparation is a revert of its integration commit. A later deployed image needs its own verified digest and version-compatible state rollback plan; reverting an image cannot be assumed to undo a state migration. Image builds, registry publication, initial state/credential provisioning, bot tests and production activation remain unperformed.
