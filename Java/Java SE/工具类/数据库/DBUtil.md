# DBUtil





```java
import java.sql.*;
import java.util.Properties;

public class DBUtil {

    private static ThreadLocal<Connection> connHolder = new ThreadLocal<>();

    //获取数据库连接
    public static Connection getConnection() {
        Connection connection = connHolder.get();
        if (connection == null) {
            try {
                //加载配置信息
                Properties properties = new Properties();
                properties.load(Thread.currentThread().getContextClassLoader().getResourceAsStream("config.properties"));
                String url = properties.getProperty("url");
                String account = properties.getProperty("account");
                String password = properties.getProperty("password");

                //获取数据库连接
                connection = DriverManager.getConnection(url, account, password);
                connHolder.set(connection);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return connection;
    }

    //开启事务
    public static void startTransition() {
        try {
            //获取数据库链接
            Connection connection = getConnection();
            //禁止自动提交事务
            connection.setAutoCommit(false);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return;
    }

    //提交事务
    public static void commit() {
        Connection connection = getConnection();
        try {
            connection.commit();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            releaseRes(null, null, connection);
        }
    }

    //回滚事务
    public static void rollback() {
        Connection connection = null;
        try {
            connection = getConnection();
            connection.rollback();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            releaseRes(null, null, connection);
        }
    }

    // 执行DQL语句，返回查询结果集；
    public static ResultSet doQuery(String sql, Object... parameters) {
        Connection connection = getConnection();
        PreparedStatement preparedStatement = null;
        try {
            //预编译sql语句；
            preparedStatement = connection.prepareStatement(sql);
            //为sql语句赋值；
            if (parameters != null) {
                for (int i = 1; i <= parameters.length; i++) {
                    preparedStatement.setObject(i, parameters[(i - 1)]);
                }
            }
            //执行sql语句并返回；
            return preparedStatement.executeQuery();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            releaseRes(null, preparedStatement, null);
        }
        return null;
    }

    //通用的执行'增删改'的操作
    public static int doUpdate(String sql, Object... parameters) {
        Connection connection = getConnection();
        PreparedStatement preparedStatement = null;
        try {
            //预编译sql语句；
            preparedStatement = connection.prepareStatement(sql);

            //遍历参数数组，为sql语句赋值；
            if (parameters != null) {
                for (int i = 1; i <= parameters.length; i++) {
                    preparedStatement.setObject(i, parameters[(i - 1)]);
                }
            }

            //执行sql语句；
            return preparedStatement.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //释放资源；
            releaseRes(null, preparedStatement, null);
        }
        return 0;
    }

    //释放资源
    public static void releaseRes(ResultSet resultSet, Statement statement, Connection connection) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (connection != null) {
            try {
                connection.close();
                connHolder.remove();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

```

