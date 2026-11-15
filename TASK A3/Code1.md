import java.util.List;

public interface Dao<T, K> {

    void insert(T object);

    T findById(K id);

    List<T> findAll();

    void update(T object);

    void delete(K id);
}
