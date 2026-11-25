# Choosing a license

**Default: MIT.** Short, permissive, widely understood. Use it unless one of the cases below applies.

| Situation                                                              | Choose                                                 |
| ---------------------------------------------------------------------- | ------------------------------------------------------ |
| Normal library, tool or template                                       | MIT                                                    |
| You want an explicit patent grant (larger projects, corporate contributors) | Apache-2.0                                        |
| Derivative work must stay open source                                  | GPL-3.0 (AGPL-3.0 if it runs as a network service)     |
| Changes to the library stay open, apps using it do not                 | MPL-2.0 or LGPL-3.0                                    |
| Content, docs or art, not code                                         | CC-BY-4.0                                              |
| No license at all                                                      | All rights reserved. Not open source. Avoid.           |

## Rules

- You can only license code you own. Code written for an employer may belong to them: confirm before publishing.
- Dependencies' licenses must be compatible with yours. Do not add GPL dependencies to an MIT project.
- To change the license, replace `LICENSE`, update the `license` field in `package.json` (SPDX identifier), and update the README.
- Keep the copyright line: `Copyright (c) <year> <name>`.
