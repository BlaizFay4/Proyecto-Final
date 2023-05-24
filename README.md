# Proyecto-Final
public class Server {
    private String metodo;
    private String url;
    private String encabezados;
    private String cuerpo;
    private int id;

    public Server(String metodo, String url, String encabezados, String cuerpo) {
        this.metodo = metodo;
        this.url = url;
        this.encabezados = encabezados;
        this.cuerpo = cuerpo;
    }

    public Server(String metodo, String url, String encabezados, String cuerpo, int id) {
        this(metodo, url, encabezados, cuerpo);
        this.id = id;
    }

    public String getMetodo() {
        return metodo;
    }

    public void setMetodo(String metodo) {
        this.metodo = metodo;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getEncabezados() {
        return encabezados;
    }

    public void setEncabezados(String encabezados) {
        this.encabezados = encabezados;
    }

    public String getCuerpo() {
        return cuerpo;
    }

    public void setCuerpo(String cuerpo) {
        this.cuerpo = cuerpo;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }
}
import java.util.regex.Matcher;
import java.util.Objects;
import java.util.Scanner;
import java.util.regex.Pattern;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class User {
private int id;
private String user;
private String pwd;
private String names;

public User(int id, String user, String pwd, String names) {
    this.id = id;
    this.user = user;
    this.pwd = pwd;
    this.names = names;
}

public int getId() {
    return id;
}

public void setId(int id) {
    this.id = id;
}

public String getUser() {
    return user;
}

public void setUser(String user) {
    this.user = user;
}

public String getPwd() {
    return pwd;
}

public void setPwd(String pwd) {
    this.pwd = pwd;
}

public String getNames() {
    return names;
}

public void setNames(String names) {
    this.names = names;
}
}

class Parser {
    private static final Pattern REQUEST_PATTERN = Pattern.compile("^(GET|POST|PUT|DELETE) /api/users(?:/(\\d+))? HTTP/1.1\\r\\nHost: localhost:(\\d+)\\r\\n(?:([\\w-]+): ([\\w ]+)\\r\\n)*\\r\\n(.*)$");

    public static Request parse(String requestString) {
        Matcher matcher = REQUEST_PATTERN.matcher(requestString);
        if (matcher.matches()) {
            String method = matcher.group(1);
            String userId = matcher.group(2);
            String port = matcher.group(3);
            String body = matcher.group(6);
            return new Request(method, userId, port, body);
        } else {
            throw new IllegalArgumentException("Invalid request string");
        }
    }
}

class BD {
private static List users = new ArrayList<>();

public static List<User> getUsers() {
    return users;
}

public static void setUsers(List<User> users) {
    BD.users = users;
}
}
class Controladora {
    public static void procesarRequest(Request request) {
        switch (request.getMethod()) {
            case "GET":
                if (request.getUserId() == null) {
                    System.out.println("Obtener todos los usuarios");
                } else {
                    System.out.println("Obtener usuario con ID " + request.getUserId());
                }
                break;
            case "POST":
                System.out.println("Crear usuario con datos " + request.getBody());
                break;
            case "PUT":
                System.out.println("Actualizar usuario con ID " + request.getUserId() + " con datos " + request.getBody());
                break;
            case "DELETE":
                System.out.println("Eliminar usuario con ID " + request.getUserId());
                break;
            default:
                System.out.println("Tipo de solicitud no válido");
                break;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        String cuerpo;
        String encabezados;
        String url;
        String version;
        int id;
        String metodo ;
        Scanner sc = new Scanner(System.in);
        // Obtén la cadena de entrada desde la consola
        System.out.println(" ingrese la linea de solicitud");
        String requestL = sc.next();

        // Define la expresión regular para la gramática BNF de las solicitudes HTTP
        String httpRegex = "^(GET|POST|PUT|DELETE)\\s(.+)\\sHTTP\\/1\\.[01]\\s(.+:.+)\\s(.*)\\s[1-9]?";
        // Compila la expresión regular en un objeto Pattern
        Pattern pattern = Pattern.compile(httpRegex);

        // un objeto matcher para comparar la linea de solicitud con el regex
        Matcher matcher = pattern.matcher(requestL);

        // Verifica si la cadena de entrada cumple con la gramática
        if (matcher.matches()) {
            System.out.println("ingrese \n1. para ver usuarios\n2. para crear un usuario\n3. para editar un usuario\n4. para eliminar un usuario\n5.para salir");
            int n = sc.nextInt();
            switch (n) {
                case 1:
                    System.out.println("1. ver lista de usuarios\n2.buscar un usuario");
                    int x = sc.nextInt();
                    switch (x) {
                        case 1:
                            metodo = matcher.group(1);
                            url = matcher.group(2);
                            version = matcher.group(3);
                            encabezados = matcher.group(4);
                            cuerpo = matcher.group(5);

                            Server pet = new Server(metodo, url, encabezados, cuerpo);
                            System.out.println(pet);
                            break;
                        case 2:
                            metodo = matcher.group(1);
                            url = matcher.group(2);
                            version = matcher.group(3);
                            encabezados = matcher.group(4);
                            cuerpo = matcher.group(5);
                            id = Integer.parseInt(matcher.group(6));
                            pet = new Server(metodo, url, encabezados, cuerpo, id);
                            System.out.println(pet);
                            break;

                    }
                case 2:
                    metodo = matcher.group(1);
                    url = matcher.group(2);
                    version = matcher.group(3);
                    encabezados = matcher.group(4);
                    cuerpo = matcher.group(5);
                    String[]u=cuerpo.split(",",4);
User US=new User(Integer.parseInt(u[0]),u[1],u[2],u[3]);
                    Server pet = new Server(metodo, url, encabezados, cuerpo);
                    System.out.println(pet);
                    break;
                case 3|4 :
                    metodo = matcher.group(1);
                    url = matcher.group(2);
                    version = matcher.group(3);
                    encabezados = matcher.group(4);
                    cuerpo = matcher.group(5);
                    id = Integer.parseInt(matcher.group(6));
                    pet = new Server(metodo, url, encabezados, cuerpo, id);
                    System.out.println(pet);
                    break;

                case 5:
                    System.out.println("adios");
                    break;


            }
        } else if (matcher.group(1) !="GET" && matcher.group(1)!="POST" && matcher.group(1)!="PUT" &&  matcher.group(1)!="DELETE"){

            System.out.println("400 mala peticion");
        }else if(matcher.group(3)!="HTTP/1.0" && matcher.group(3)!="HTTP/1.1"){
            System.out.println("404 recurso no encontrado");
        } else if(matcher.group(5).isEmpty()){
            System.out.println("500 solicitud no procesada");
        }

    }
}
