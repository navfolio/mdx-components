# @navfolio/mdx-components

Official Astro components for Navfolio MDX content. This package is intentionally separate from `@navfolio/plugin-markdown`: the plugin configures Markdown rendering, while this package supplies author-facing blocks such as friend links, a friend circle, carousels, long images, Mermaid initialization, and zoomable images.

## Install

```bash
bun add @navfolio/mdx-components@github:navfolio/mdx-components
```

Then import only the component needed by an MDX file:

```astro
import FriendLinkCard from '@navfolio/mdx-components/FriendLinkCard.astro';
import FriendCircle from '@navfolio/mdx-components/FriendCircle.astro';
```

## Friend links

`FriendLinkCard` accepts inline data or a JSON endpoint. `src` wins by default and falls back to `items` if the remote JSON fails. Use `source="src"` or `source="items"` to force one source.

```astro
<FriendLinkCard
  columns={4}
  randomBackground={true}
  shuffle={true}
  items={[
    {
      name: 'Example',
      url: 'https://example.com',
      bio: 'A short description.',
      avatar: 'https://example.com/avatar.png',
      backgroundImage: 'https://example.com/banner.jpg',
      rss: 'https://example.com/feed.xml',
      sticky: 10,
    },
  ]}
/>
```

Desktop uses four columns by default and naturally contracts to three, two, then one column. Bios clamp to two lines and cards keep a stable minimum height. `sticky` accepts `true` or a positive number; higher numbers render first. `shuffle` affects only non-sticky links. When `randomBackground` is enabled, each card receives a server-rendered translucent color, so there is no client-side color flash and every page refresh gets a fresh combination.

## Friend circle

`FriendCircle` renders a compact static feed below the links. Add `rss` to friend data and it crawls RSS/Atom feeds at build time. The refresh button samples already-built data in the browser; it never creates cross-origin requests for visitors.

For production, periodically generate a JSON file with GitHub Actions and pass it with `feedSrc="/friend-circle.json"`. This avoids deployment failures from unreachable feeds and preserves the same component API for a Vercel Function or self-hosted service later.

## Peer dependencies

The host Astro project supplies `astro`, `lucide-astro`, and `mermaid` (the latter is only required for `MermaidRenderer`).
