import java.io.Serializable;

import java.util.ArrayList;

import java.util.List;

class Resident implements Serializable {

    private static final long serialVersionUID = 1L;

    String name;
    String registerNumber;

    transient String password;
    transient java.sql.Connection connection;
}

class Room implements Serializable {

    private static final long serialVersionUID = 1L;

    List<Resident> residents = new ArrayList<>();
}

class Hostel implements Serializable {

    private static final long serialVersionUID = 1L;

    List<Room> rooms = new ArrayList<>();
}
