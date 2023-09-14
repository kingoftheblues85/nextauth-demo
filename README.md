## Description

The purpose of this app is to demonstrate how to set up nextauth

1. First, set up an account at https://console.cloud.google.com/

2. Install the next-auth package
```
npm install next-auth
```

3. Set up the environment variables. 
```
> ./.env
GOOGLE_ID='<google-id-goes-here>'
GOOGLE_SECRET='<google-secret-key-goes-here>'
```

4. Set up the API route 
```
> ./app/api/auth/[...nextauth]/route.ts

import NextAuth from "next-auth"
const handler = NextAuth({
  providers: [
      GoogleProvider({
        clientId: process.env.GOOGLE_ID,
        clientSecret: process.env.GOOGLE_SECRET
      }),
    ]
})
export { handler as GET, handler as POST }
```

5. Create a provider file to wrap the entire layout in
```
> ./providers/NextAuthSessionProvider.tsx:

"use client";
import { SessionProvider } from "next-auth/react";
export default function NextAuthSessionProvider({
  children,
}: {
  children: React.ReactNode;
}) {
  return <SessionProvider>{children}</SessionProvider>;
}
```

6. Wrap the contents of the body tag in layout.tsx with the session provider
```
> ./app/layout.tsx
...
<body className={inter.className}>
  <NextAuthSessionProvider>
    <Nav />
    {children}
  </NextAuthSessionProvider>
</body>
...
```

7. To use the session:
`import { useSession, signIn, signOut } from 'next-auth/react'`
`const { data: session } = useSession();`

8. To chech for session:
`{session ? session.user : null}`
