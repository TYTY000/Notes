int
compare_doubles (const void \*a, const void \*b)
{
  const double \*da = (const double \*) a;
  const double \*db = (const double \*) b;

  return (\*da > \*db) - (\*da < \*db);
}