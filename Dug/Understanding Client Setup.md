[[dug]]

There are two types of client: HPC and OnG. OnG use Insight, HPC don't


Client setup step as follow
1. On Zendesk, assign the organization as McCloud. Now their ticket will go to FLS instead of Insight
2. Create LDAP group. Make sure the gid is unique. Then according to customer type, add support people into the group. svc_resourcemanager is also added to track usage.
3. Add McCloud users (will be done through portal now)
4. Create VAST storage for that org
5. Mount that storage on non-root squash machine, create home directory, and chgrp it.
6. According to the customer type, copy 