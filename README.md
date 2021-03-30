custom-user-storage-jpa: Custom User Storage Provider for RHSSO / Keycloak with EJB and JPA
============================================================================================

Refer this video https://youtu.be/3m5aEQFGr6U for the implementation details of this repo.

Sample datasource configuration that's used in this sample can be found here - https://gist.githubusercontent.com/bbalakriz/974def1b0d286ae5ba8e0c3fca507a80/raw/b41e3c6b878dd5ec72af5fdefcd19110d8905488/keycloak-standalone.xml. 


Default list all users won't work in the keycloak admin console, when using user storage SPI.

update below method , which will work as expected to list all users.

     @Override
     public List<UserModel> searchForUser(Map<String, String> params, RealmModel realm,
            int firstResult, int maxResults) {
     
        Query query = em.createNativeQuery(UserStoreQueries.GET_ALL_USERS);

       
        if (firstResult != -1) {
            query.setFirstResult(firstResult);
        }
        if (maxResults != -1) {
            query.setMaxResults(maxResults);
        }
        List<Object[]> results = query.getResultList();
        List<UserModel> users = new LinkedList<>();
        for (Object[] entity : results)
            users.add(new UserAdapter(kcSession, realm, model, prepareUserEntity(entity)));
            return users;
      }
