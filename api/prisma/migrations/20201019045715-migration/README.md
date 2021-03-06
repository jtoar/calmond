# Migration `20201019045715-migration`

This migration has been generated by dom at 10/18/2020, 9:57:15 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "Project" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    "name" TEXT NOT NULL
)

CREATE TABLE "Day" (
    "createdAt" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" DATETIME NOT NULL,
    "projectName" TEXT NOT NULL,
    "date" DATETIME NOT NULL,
    "hasEntry" BOOLEAN NOT NULL DEFAULT true,
    "notes" TEXT NOT NULL DEFAULT '',

    FOREIGN KEY ("projectName") REFERENCES "Project"("name") ON DELETE CASCADE ON UPDATE CASCADE,
PRIMARY KEY ("projectName","date")
)

CREATE UNIQUE INDEX "Project.name_unique" ON "Project"("name")
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20201019045715-migration
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,29 @@
+datasource DS {
+  provider = ["sqlite", "postgresql"]
+  url = "***"
+}
+
+generator client {
+  provider      = "prisma-client-js"
+  binaryTargets = "native"
+}
+
+model Project {
+  id        Int      @id @default(autoincrement())
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  name      String   @unique
+  days      Day[]
+}
+
+model Day {
+  createdAt   DateTime @default(now())
+  updatedAt   DateTime @updatedAt
+  project     Project  @relation(fields: [projectName], references: [name])
+  projectName String
+  date        DateTime
+  hasEntry    Boolean  @default(true)
+  notes       String   @default("")
+
+  @@id([projectName, date])
+}
```


