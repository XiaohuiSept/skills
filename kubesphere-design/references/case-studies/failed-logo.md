---
status: active
---

# Failed Logo

## Wrong Output

The header uses text-only `KubeSphere`, a generated mark, a missing image placeholder, or
`LogoText Kube Sphere`.

## Why It Fails

The product frame becomes unrecognizable before the page content is evaluated. KubeSphere
branding is a hard visual anchor.

## Correct Pattern

Use:

```tsx
<img src="https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg" alt="KubeSphere" />
```

## Checklist

- Logo is an image, not text.
- URL matches the official logo URL.
- Brand area remains `224px`.
