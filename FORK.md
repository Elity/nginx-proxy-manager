# Fork publishing

Development: develop. The upstream contribution branch contains only application,
UI, test and documentation changes; fork image files stay on develop.

The OIDC image workflow builds from the official 2.15.1 runtime and replaces its
backend/frontend and matching develop rootfs with this fork. Runtime base image is pinned by digest. It
publishes linux/amd64 and linux/arm64 at ghcr.io/elity/nginx-proxy-manager:oidc
and oidc-<commit>. Deploy immutable digests with the existing /data and
/etc/letsencrypt mounts. Back up both directories before migrations.

Do not run two instances against the same database or bind the same proxy ports.
Test candidates on copies of the data before replacing a production instance.
Rollback must account for database migrations and preserve edits made after
deployment; never blindly restore a whole old database over new proxy changes.

OIDC account linking requires an existing local account, its password and any
enabled NPM two-factor code. Configuring an identity provider never implicitly
grants its users administrator privileges.
